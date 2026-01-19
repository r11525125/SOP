Figure0119 - Plot Package

Overview
This folder contains regenerated plots (scheduling results + SOP policy comparison).
All figures are created from existing ns-3 outputs; no synthetic data is used.

Data Sources
- Scheduling (Figures 10-14):
  /home/adlink/electronics/實驗資料/ns-allinone-3.42/ns-3.42/results/sop
  Seeds: cases 1-4 use seeds 1-3; scheduling experiment uses seeds 1-5.
- SOP+Link-Quality:
  /home/adlink/electronics/ns-3-dev/ml_results/radio_suite2/sop_ml_full
  Scenario label: SOP_ML_RADIO
  Seeds: 1-3; UEs: 6, 8, 10, 20, 30
- SOP+LSTM (alloc-only):
  /home/adlink/electronics/ns-3-dev/ml_results/lstm_suite/lstm_runs_alloc_only
  Scenario label: SOP_ML_LSTM_ALLOC
  Seed: 3; UEs: 6, 8, 10, 20, 30
- Always MEC/Remote:
  /home/adlink/electronics/ns-3-dev/ml_results/radio_suite2/always_mec
  /home/adlink/electronics/ns-3-dev/ml_results/radio_suite2/always_remote
  Seeds: 1-3; UEs: 6, 8, 10, 20, 30

Figures
- fig_10_active_cases.png: Active uploading vehicles over time (Case 1-4).
- fig_11_completion_dist.png: Completion time distribution (Case 1-4).
- fig_12_completion_cdf.png: CDF of completion times (Case 1-4).
- fig_13_sched_cdf.png: CDF of upload completion times for Method 1-6.
- fig_14_sched_active.png: Waiting vehicles over time for Method 1-6.
- fig_lstm_delay.png: SOP vs SOP+LSTM (alloc-only) delays (video/sensor/bulk; three subplots).
- fig_lstm_bulk.png: SOP vs SOP+LSTM (alloc-only) throughput (video/sensor/bulk; three subplots).
- fig_policy_delay.png: SOP+LSTM (alloc-only) vs Always MEC/Remote delays (video/sensor/bulk; three subplots).
- fig_policy_bulk.png: SOP+LSTM (alloc-only) vs Always MEC/Remote throughput (video/sensor/bulk; three subplots).
- lstm_best_agg.csv: Aggregated SOP vs SOP+LSTM (alloc-only) metrics (mean + standard error).
- policy_comparison_agg.csv: Aggregated policy comparison metrics (mean + standard error).

Reproduce
Run the plotting script:
  python3 /home/adlink/electronics/plot_figures_0119.py

Slides
- /home/adlink/electronics/figure0119/slides.md
- /home/adlink/electronics/figure0119/slides_zh.md

Notes
- Scheduling figures reuse the original plotting style and seeds from the SOP scheduler runs.
- Policy comparison plots use only seed=3 for all policies to match the LSTM t60 runs.

Policy Comparison Details
- SOP+LSTM (alloc-only) comes from lstm_suite/lstm_runs_alloc_only (Scenario: SOP_ML_LSTM_ALLOC).
- Always MEC/Remote come from radio_suite2/always_mec and radio_suite2/always_remote.
- To keep seeds consistent with LSTM alloc-only, the comparison plots only use seed=3 and UE counts
  {8, 10, 20, 30} for all policies (UE6 excluded).

Workflow and Method Relationships (ASCII Summary)
- SOP is the system architecture (SOT tracking + SOS proxy + MEC/remote data upload).
- Link-Quality and LSTM are decision modules inside SOP that decide MEC vs Remote and
  adjust flow rates based on predicted connectivity quality.
- The six methods are SOS-side bandwidth scheduling rules for queued upload jobs; they
  do not perform offloading decisions and are evaluated separately in Figures 13-14.

Suggested Caption Text (English, ASCII)
- Fig. 10: Active uploading vehicles over time for SOP (Method 3) vs FIFO baseline under
  four backhaul delay/loss cases; lower curves indicate faster completion.
- Fig. 11: Distribution of completion times under four network cases; the dashed line
  marks the 30 s dwell-time threshold.
- Fig. 12: CDF of completion times under four cases; SOP shifts the CDF left compared to
  FIFO baseline, indicating higher on-time completion probability.
- Fig. 13: CDF of upload completion times for SOS scheduling Methods 1-6; each method
  combines a job-selection rule with a bandwidth allocation policy.
- Fig. 14: Waiting (active) vehicles over time for Methods 1-6; lower queues indicate
  better scheduling under bursty arrivals.
- Fig. 15 (policy delay): SOP+Link-Quality (radio-score) vs SOP+LSTM (t60) vs Always MEC/Remote
  for video/sensor delay; policies share the same UE counts and seed=3.
- Fig. 16 (policy bulk): SOP+Link-Quality vs SOP+LSTM (t60) vs Always MEC/Remote for bulk
  throughput and delay; policies share the same UE counts and seed=3.
# SOP
