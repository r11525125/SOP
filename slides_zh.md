% SOP 實驗投影片（中文）

# SOP 實驗結果（中文）

- 資料來源：全部為 ns-3 真實輸出結果（無合成數據）
- UE 組合（政策/LSTM 圖）：{8, 10, 20, 30}，seed=3（UE6 排除）
- 圖片與說明對應 `figure0119` 內檔案

---

# 內容大綱

- 架構與流程
- 卸載決策與距離門檻
- 六種排程方法
- 實驗設定與指標
- 結果與討論
- 侷限與未來工作

---

# 系統架構（SOP）

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_architecture.png)

- SOP = SOT（追蹤/建議）+ SOS（代理/緩衝/排程）+ MEC/Remote 上傳
- 車輛上傳先進 SOS，再由 SOS 轉送或上傳至目標伺服器
- 排程只發生在 SOS 端（與卸載決策分離）

---

# SOP 流程（文字版）

```
1. 車輛發起上傳請求
2. SOT 選擇合適的 SOS（地理位置 / 負載 / 頻寬）
3. 車輛連接到 SOS 開始上傳
4. SOS 以 Method 1-6 之一分配頻寬（Section 4.2 評估）
5. SOS 將資料傳送到目標伺服器（MEC 或遠端）
```

---

# 模擬環境（概念圖）

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_sim_env.png)

- 5G gNodeB、MEC+SOS、SOT、Remote Server
- 回程（Backhaul）延遲/丟包可調，用於 case 測試

---

# 卸載決策（預設 SOP）

- 決策分數（距離門檻）  
  score = 1 - (distance_to_closest_gNB / maxRange)
- 預設 maxRange = 500 m
- 預設 poorThreshold = 0.4  
  - score < 0.4 → 走 MEC  
  - score ≥ 0.4 → 走 Remote
- 這是 SOP 預設規則（未啟用 ML/LSTM）

---

# Link‑Quality 與 LSTM 的位置

- SOP 架構不變
- Link‑Quality 或 LSTM 只取代「決策分數」
  - Link‑Quality：RSRP/RSRQ/SINR/CQI 加權 + EWMA
  - LSTM：用時間序列預測連線品質
- LSTM alloc‑only：只影響上傳分配（allocFactor），不改 MEC/Remote 決策

---

# 六種排程方法（SOS 端）

六種方法 = 「工作選擇規則 × 帶寬分配策略」

- 選擇規則（3 種）
  - All Jobs
  - Shortest k Jobs (by execution time)
  - Shortest k Jobs (by remaining time)
- 分配策略（2 種）
  - Equal Bandwidth
  - Longest Remaining First

這些方法只影響 SOS 端排程，不影響卸載決策。

---

# 指標與統計

- 完成時間分布 / CDF
- Active/Waiting Vehicles over Time
- Mean Delay / Throughput
- P50 / P90 / P99 / Within Dwell‑time

---

# 實驗 A：SOP vs FIFO（回程條件）

設定摘要
- 檔案大小：10 MB
- k=5（Method 3）
- 回程 Case：
  - (0 ms, 0)
  - (0 ms, 0.01)
  - (50 ms, 0)
  - (50 ms, 0.01)

指標：Active Vehicles、Completion PDF/CDF

---

# Fig.10：Active Vehicles over Time

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_10_active_cases.png)

- SOP (Method 3) 相較 FIFO 降低排隊壓力

---

# Fig.11：完成時間分布

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_11_completion_dist.png)

- 30 秒虛線為 dwell‑time 門檻

---

# Fig.12：完成時間 CDF

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_12_completion_cdf.png)

- SOP 曲線左移，準時完成比例較高

---

# 實驗 B：六種排程方法

設定摘要
- SOS link：1 Gbps, 50 ms, loss 0.01
- Poisson arrivals + burst window
- dwell‑time = 30 s

指標：CDF、Waiting Vehicles

---

# Fig.13：排程方法 CDF

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_13_sched_cdf.png)

- 不同方法影響 tail latency

---

# Fig.14：Waiting Vehicles

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_14_sched_active.png)

- 等待車輛越少代表排程效率越高

---

# 實驗 C：政策比較（不含 Link‑Quality）

Policies
- SOP+LSTM (alloc‑only)
- Always MEC
- Always Remote

設定
- UE = {8, 10, 20, 30}
- seed = 3

---

# Fig.15：政策 Delay（Video/Sensor/Bulk）

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_policy_delay.png)

---

# Fig.16：政策 Throughput（Video/Sensor/Bulk）

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_policy_bulk.png)

---

# 實驗 D：SOP vs SOP+LSTM（alloc‑only）

Policies
- SOP（距離門檻）
- SOP+LSTM（alloc‑only）

設定
- UE = {8, 10, 20, 30}
- seed = 3

---

# Fig.17：SOP vs LSTM Delay

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_lstm_delay.png)

---

# Fig.18：SOP vs LSTM Throughput

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_lstm_bulk.png)

---

# 貢獻總結

- 完整 SOP 架構與 SOP‑SOS 排程系統化設計
- 六種排程方法揭示排隊與 tail latency 的 trade‑off
- LSTM/Link‑Quality 作為 SOP 的決策模組（可交換）
- 實驗完整呈現多種網路條件與策略比較

---

# 限制與未來工作

- UE6 於 LSTM alloc‑only 呈現 outlier，本版圖表排除
- LSTM alloc‑only 不改卸載決策，只影響分配
- 可補充：多 seed 平均、與 SOP+Link‑Quality 的完整比較
