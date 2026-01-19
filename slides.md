% SOP Experiment Slides (figure0119)

# SOP Experiment Results (figure0119)

- Dataset: ns-3 results only (no synthetic data)
- UE set for policy/LSTM plots: {8, 10, 20, 30} (UE6 excluded)
- Seed for policy/LSTM plots: 3

---

# Architecture Overview

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_architecture.png)

- SOP = SOT (tracker) + SOS (proxy) + MEC/Remote upload
- SOS buffers and schedules uploads
- Decision module selects MEC vs Remote and adapts rates

---

# Simulation Environment

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_sim_env.png)

- 5G gNodeB + MEC+SOS + SOT + Remote server
- Backhaul delay/loss varied per case (0/50 ms, 0/0.01)

---

# Offloading Decision (Default SOP)

- Decision score: score = 1 - (distance_to_closest_gNB / maxRange)
- Default maxRange = 500 m
- Default threshold: poorThreshold = 0.4
  - If score < 0.4, use MEC; else use Remote
- ML/LSTM replaces the score with predicted connectivity

---

# Contributions (Summary)

- SOP architecture for stable upload under mobility
- Six SOS scheduling methods (selection rule x bandwidth allocation)
- Link-quality and LSTM decision modules (trade-offs shown)

---

# Experiment A: SOP vs FIFO (Backhaul Cases)

Settings
- Upload size: 10 MB
- k = 5 (shortest-k selection in Method 3)
- Cases: (0 ms, 0), (0 ms, 0.01), (50 ms, 0), (50 ms, 0.01)

Metrics
- Active vehicles over time
- Completion time distribution and CDF

---

# Fig. 10: Active Vehicles Over Time

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_10_active_cases.png)

- SOP (Method 3) reduces backlog vs FIFO baseline

---

# Fig. 11-12: Completion Time Distribution and CDF

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_11_completion_dist.png)

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_12_completion_cdf.png)

- SOP shifts completion times earlier under all cases

---

# Experiment B: Six Scheduling Methods

Settings
- SOS link: 1 Gbps, 50 ms, loss 0.01
- Poisson arrivals with burst window
- Dwell time threshold: 30 s

Metrics
- Completion time CDF
- Waiting vehicles over time

---

# Fig. 13: Scheduling CDF (Methods 1-6)

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_13_sched_cdf.png)

- Different method combinations change tail latency

---

# Fig. 14: Waiting Vehicles Over Time

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_14_sched_active.png)

- Lower queues imply better scheduling under bursty loads

---

# Experiment C: Policy Comparison

Policies
- SOP+LSTM (alloc-only)
- Always MEC
- Always Remote

Settings
- UE set {8, 10, 20, 30}, seed=3
- Same traffic profile as SOP baseline

---

# Fig. 15: Policy Delay (Video/Sensor/Bulk)

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_policy_delay.png)

---

# Fig. 16: Policy Throughput (Video/Sensor/Bulk)

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_policy_bulk.png)

---

# Experiment D: SOP vs SOP+LSTM (alloc-only)

Policies
- SOP (default distance threshold)
- SOP+LSTM (alloc-only)

Settings
- UE set {8, 10, 20, 30}, seed=3

---

# Fig. 17: SOP vs LSTM Delay

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_lstm_delay.png)

---

# Fig. 18: SOP vs LSTM Throughput

![](https://raw.githubusercontent.com/r11525125/SOP/main/fig_lstm_bulk.png)

---

# Notes and Limitations

- UE6 excluded from policy/LSTM plots due to outlier behavior
- LSTM alloc-only affects rate allocation, not MEC/Remote decision
- Results are seed=3 for LSTM/Policy comparisons
