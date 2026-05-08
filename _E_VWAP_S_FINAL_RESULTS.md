# E_VWAP_S 최적화 최종 결과

**110 라운드** (10 + 100) + 차등투자 알고리즘 통합 결과

## 5 챔피언 요약

| # | 라운드 | 핵심 기법 | FULL NET | FULL 승률 | 비고 |
|---|---|---|---|---|---|
| 🥇 NET 1위 | 10R-R6 | 시초 갭 ≤ 3% | +8.12% | 61.9% | 등액 |
| 🥇 WIN 1위 | 10R-R5 | VWAP 연속 + 작은 익절 +2% | +0.40% | **86.3%** | 등액 |
| 💎 차등 NET 1위 | 100R-R59 | R6 + 차등=COMBINED | +9.77% | 83.2% | 가중. max 44.05% |
| 🏆 등액 NET (100R) | R99 | R6 + 부분청산(50%@+2%) + score | +4.20% | 67.8% | 등액 |
| 📊 HOLD NET | R30 | R6 갭=5% (완화) | hold +3.06% | 60.53% | 안정성 |

## 산출물

- HTML 종합: `_E_VWAP_S_FINAL_RESULTS.html` (GitHub 업로드용)
- HTML 10라운드: `_E_VWAP_S_10라운드_최적화_20260508.html`
- HTML 100라운드: `_E_VWAP_S_100ROUND_RESULTS.html`
- CSV 100라운드: `reports/evwap_s_optimization/rounds100_all_trials.csv`
- JSON 챔피언: `reports/evwap_s_optimization/rounds100_best.json`