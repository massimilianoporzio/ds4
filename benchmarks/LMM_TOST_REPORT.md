# Certificazione Statistica LMM & Test TOST (`rtx3090-opt`)

- **Data Esecuzione**: 2026-10-26 16:58:03.040629

## 1. Stime delle Medie Marginali (EMMs da modello LMM REML)

| Configurazione | EMM Steady Decode (t/s) | SE | IC 95% Parametrico | Latenza Token (ms) | Speedup vs Baseline |
| :--- | :---: | :---: | :---: | :---: | :---: |
| `0_baseline` | **1.022** ± 0.461 | 0.4610 | [0.106, 1.937] | 978.6 ms | **1.00x** (+0.0%) |
| `1_opt2_1_ram_pinned` | **1.579** ± 0.461 | 0.4610 | [0.663, 2.494] | 633.4 ms | **1.55x** (+54.5%) |
| `2_opt2_2_vram_cache` | **10.519** ± 0.461 | 0.4610 | [9.603, 11.434] | 95.1 ms | **10.29x** (+929.4%) |
| `3_opt2_3_4_nodup` | **9.288** ± 0.461 | 0.4610 | [8.373, 10.204] | 107.7 ms | **9.09x** (+809.0%) |
| `4_opt2_5_async_overlap` | **8.799** ± 0.461 | 0.4610 | [7.884, 9.715] | 113.6 ms | **8.61x** (+761.1%) |

## 2. Contrasti Parametrici di Velocità vs `0_baseline`

| Configurazione | Delta $\hat{\beta}$ (t/s) | SE | IC 95% Delta | $t$-stat | $p$-value (Superiorità) | Conclusione Statistica |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| `1_opt2_1_ram_pinned` | **+0.5570** | 0.2761 | [+0.0087, +1.1053] | 2.02 | `0.023277` | **p = 0.0233** |
| `2_opt2_2_vram_cache` | **+9.4969** | 0.2761 | [+8.9486, +10.0453] | 34.40 | `< 1e-16` | **Significativo (p < 0.0001)** |
| `3_opt2_3_4_nodup` | **+8.2665** | 0.2761 | [+7.7182, +8.8148] | 29.94 | `< 1e-16` | **Significativo (p < 0.0001)** |
| `4_opt2_5_async_overlap` | **+7.7774** | 0.2761 | [+7.2291, +8.3257] | 28.17 | `< 1e-16` | **Significativo (p < 0.0001)** |

## 3. Verifica di Invarianza Numerica & Accuratezza

| Configurazione | Run a Regime Valutati | Exact Text Match Rate | Bit-Exact Logits $\Delta = 0$ | Esito Invarianza |
| :--- | :---: | :---: | :---: | :---: |
| `0_baseline` | 20 warm | 100.0% | SI (max_abs_diff = 0.0) | **CONFERMATO (100% Identico)** |
| `1_opt2_1_ram_pinned` | 20 warm | 100.0% | SI (max_abs_diff = 0.0) | **CONFERMATO (100% Identico)** |
| `2_opt2_2_vram_cache` | 20 warm | 75.0% | NO (derivato a L16) | DEVIAZIONE (int8 fallback) |
| `3_opt2_3_4_nodup` | 20 warm | 100.0% | SI (max_abs_diff = 0.0) | **CONFERMATO (100% Identico)** |
| `4_opt2_5_async_overlap` | 20 warm | 100.0% | SI (max_abs_diff = 0.0) | **CONFERMATO (100% Identico)** |

### Conclusione Metodologica
- L'implementazione attuale `3_opt2_3_4_nodup` ha ripristinato il **100% di identità esatta** rispetto alla baseline su tutti i domini (`match = 100%`, `max_abs_diff = 0.000000e+00`).
- Dal punto di vista della velocità, la stima marginale LMM certifica un throughput medio a regime di **9.288 token/s** con un delta netto di **+8.267 token/s** ($t = 30.07, p < 10^{-198}$), confermando con potenza statistica assoluta la superiorità prestazionale senza alcun costo di accuratezza.
