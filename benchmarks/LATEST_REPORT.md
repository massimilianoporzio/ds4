# Report Statistico Benchmark (`rtx3090-opt`)

## 1. Statistiche Descrittive e Non-Parametriche per Dominio (Run a Regime `is_cold=0`)

| Config | Dominio | N (Warm) | Prefill (t/s) $\mu \pm \sigma$ | Decode Steady (t/s) $\mu \pm \sigma$ | Mediana (IQR) | $\text{IC}_{95\%}$ Bootstrap | Latenza Token (ms) | Slot VRAM (GiB) | Match |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| `0_baseline` | `D1_code` | 5 | 4.08 ± 0.03 | **1.007** ± 0.018 | 1.012 (0.019) | [0.992, 1.020] | 993.0 ms | 199 (1.310G) | OK |
| `0_baseline` | `D2_math` | 5 | 5.47 ± 0.04 | **1.020** ± 0.004 | 1.021 (0.007) | [1.017, 1.023] | 980.4 ms | 197 (1.300G) | OK |
| `0_baseline` | `D3_prose` | 5 | 4.23 ± 0.02 | **1.016** ± 0.008 | 1.019 (0.009) | [1.009, 1.022] | 984.1 ms | 204 (1.340G) | OK |
| `0_baseline` | `D4_bench` | 5 | 1.03 ± 0.03 | **1.044** ± 0.030 | 1.040 (0.050) | [1.020, 1.068] | 958.5 ms | 199 (1.310G) | OK |
| `1_opt2_1_ram_pinned` | `D1_code` | 5 | 5.57 ± 0.04 | **1.597** ± 0.033 | 1.586 (0.033) | [1.574, 1.621] | 626.2 ms | 199 (1.310G) | OK |
| `1_opt2_1_ram_pinned` | `D2_math` | 5 | 7.44 ± 0.02 | **1.585** ± 0.020 | 1.571 (0.027) | [1.571, 1.601] | 631.1 ms | 197 (1.300G) | OK |
| `1_opt2_1_ram_pinned` | `D3_prose` | 5 | 5.71 ± 0.03 | **1.579** ± 0.016 | 1.574 (0.020) | [1.567, 1.591] | 633.3 ms | 204 (1.340G) | OK |
| `1_opt2_1_ram_pinned` | `D4_bench` | 5 | 1.56 ± 0.01 | **1.554** ± 0.048 | 1.530 (0.010) | [1.530, 1.596] | 644.0 ms | 199 (1.310G) | OK |
| `2_opt2_2_vram_cache` | `D1_code` | 5 | 8.76 ± 0.03 | **11.639** ± 0.453 | 11.476 (0.137) | [11.384, 12.039] | 86.0 ms | 1801 (11.870G) | DIFF |
| `2_opt2_2_vram_cache` | `D2_math` | 5 | 11.57 ± 0.28 | **9.355** ± 0.151 | 9.430 (0.090) | [9.222, 9.448] | 106.9 ms | 1796 (11.840G) | OK |
| `2_opt2_2_vram_cache` | `D3_prose` | 5 | 8.78 ± 0.07 | **11.647** ± 0.142 | 11.655 (0.219) | [11.537, 11.758] | 85.9 ms | 1802 (11.880G) | OK |
| `2_opt2_2_vram_cache` | `D4_bench` | 5 | 32.89 ± 0.15 | **9.434** ± 0.645 | 9.180 (1.060) | [8.936, 9.932] | 106.4 ms | 1813 (11.950G) | OK |
| `3_opt2_3_4_nodup` | `D1_code` | 5 | 8.89 ± 0.06 | **9.516** ± 0.453 | 9.425 (0.663) | [9.169, 9.863] | 105.3 ms | 1510 (9.950G) | OK |
| `3_opt2_3_4_nodup` | `D2_math` | 5 | 12.00 ± 0.07 | **9.220** ± 0.254 | 9.182 (0.098) | [9.058, 9.444] | 108.5 ms | 1505 (9.920G) | OK |
| `3_opt2_3_4_nodup` | `D3_prose` | 5 | 8.93 ± 0.06 | **11.409** ± 0.337 | 11.194 (0.618) | [11.157, 11.659] | 87.7 ms | 1510 (9.950G) | OK |
| `3_opt2_3_4_nodup` | `D4_bench` | 5 | 33.63 ± 0.19 | **7.008** ± 1.448 | 6.350 (1.720) | [5.986, 8.258] | 147.1 ms | 1521 (10.030G) | OK |

## 2. Sintesi Globale Multi-Dominio (Media su tutti i Run a Regime + Cold Start)

| Config | N Tot (Warm) | Cold Decode (t/s) | Warm Decode $\mu \pm \sigma$ (t/s) | Mediana (IQR) | $\text{IC}_{95\%}$ Bootstrap | Speedup vs Baseline |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| `0_baseline` | 20 | 1.004 | **1.022** ± 0.022 | 1.021 (0.010) | [1.013, 1.032] | **1.00x** (+0.0%) |
| `1_opt2_1_ram_pinned` | 20 | 1.548 | **1.579** ± 0.033 | 1.574 (0.040) | [1.565, 1.593] | **1.55x** (+54.5%) |
| `2_opt2_2_vram_cache` | 20 | 10.246 | **10.519** ± 1.213 | 10.756 (2.128) | [10.012, 11.031] | **10.29x** (+929.4%) |
| `3_opt2_3_4_nodup` | 20 | 7.583 | **9.288** ± 1.757 | 9.231 (1.347) | [8.490, 10.014] | **9.09x** (+809.0%) |

## 3. Modello Lineare a Effetti Misti (LMM REML): `steady_decode_tps ~ config + (1 | domain)`

- **Componenti di Varianza (Effetti Aleatori)**: $\hat{\sigma}_{\text{domain}} = 0.6980\text{ t/s}$, $\hat{\sigma}_{\text{residual}} = 0.8694\text{ t/s}$, $\text{ICC} = 39.2\%$, $N = 80, \text{df} = 76$

| Parametro Effetto Fisso | Stima $\hat{\beta}$ (t/s) | Errore Standard (SE) | $\text{IC}_{95\%}$ Parametrico | $t$-stat | $p$-value |
| :--- | :---: | :---: | :---: | :---: | :---: |
| Intercetta (`0_baseline`) | **1.0219** | 0.3995 | [0.2389, 1.8049] | 2.56 | 1.053e-02 |
| Delta `1_opt2_1_ram_pinned` vs Baseline | **+0.5570** | 0.2749 | [0.0182, 1.0958] | 2.03 | 4.276e-02 |
| Delta `2_opt2_2_vram_cache` vs Baseline | **+9.4969** | 0.2749 | [8.9581, 10.0358] | 34.55 | 1.686e-261 |
| Delta `3_opt2_3_4_nodup` vs Baseline | **+8.2665** | 0.2749 | [7.7276, 8.8053] | 30.07 | 1.220e-198 |
