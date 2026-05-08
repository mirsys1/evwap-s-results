# E_VWAP_S 백테스트 최적화 결과

키움증권 1분봉 데이터로 한국 주식 단타 전략 **E_VWAP_S** 를 110 라운드 / 17,000+ trial 최적화한 결과 모음.

데이터: F:/KiwoomData (9개 조건검색 폴더 × 40 unique 거래일 × 3,572 종목·일).

---

## 🌐 GitHub Pages

이 리포지터리에 GitHub Pages가 활성화되면 다음 URL에서 직접 열람 가능:
- 종합: `index.html` ← **여기부터 보세요**
- 10 라운드 단독: `10round.html`
- 100 라운드 + 차등투자: `100round.html`
- 최고 승률 전략 상세: `best_winrate.html`
- 144 조합 백테스트 heatmap: `144combos_heatmap.html`

---

## 🏆 5 챔피언 요약

| 카테고리 | 라운드 | 핵심 기법 | FULL NET | FULL 승률 | 비고 |
|---|---|---|---|---|---|
| 🥇 **NET 1위** | 10R-R6 | 시초 갭 ≤ 3% | **+8.12%** | 61.9% | 등액 |
| 🥇 **WIN 1위** | 10R-R5 | VWAP 연속 + 작은 익절 +2% | +0.40% | **86.32%** | 등액. HOLD 89.7% |
| 💎 **차등 NET 1위** | 100R-R59 | R6 + COMBINED 가중치 | **+9.77% (가중)** | 64.59% (가중) | max pos 44% — cap 필수 |
| 🏆 **등액 NET 1위 (100R)** | R99 | R6 + 부분청산(50%@+2%) + score | +4.20% | 67.8% | 등액 |
| 📊 **HOLD NET 1위** | R30 | R6 갭=5% (완화) | hold +3.06% | hold 53.0% | holdout 안정 |

---

## 📊 베이스라인 대비 개선

| 지표 | Baseline | 최고 챔피언 | 개선폭 |
|---|---|---|---|
| FULL NET (등액) | +2.67% | **+8.12%** (R6) | **+5.45%p** (3.0배) |
| FULL NET (가중) | n/a | **+9.77%** (R59) | **+7.10%p** (3.7배) |
| FULL 승률 | 48.9% | **86.32%** (R5) | **+37.4%p** (1.77배) |
| HOLDOUT NET | +0.51% | **+3.29%** (R6) | **+2.78%p** (6.4배) |
| HOLDOUT 승률 | 44.2% | **89.74%** (R5) | **+45.5%p** (2.03배) |

---

## 📁 파일 구조

```
evwap-s-results/
├─ index.html                      # 🏠 종합 결과 (110 라운드 + 차등투자)
├─ 10round.html                    # Part 1: 10 라운드
├─ 100round.html                   # Part 2: 100 라운드 + 차등투자
├─ best_winrate.html                # WIN 1위 전략 상세
├─ 144combos_heatmap.html           # 144 조합 백테스트 heatmap
├─ README.md                        # 본 파일
└─ _E_VWAP_S_FINAL_RESULTS.{html,md}  # 한글 파일명
└─ _E_VWAP_S_*.{html,md}             # 한글 파일명
```

---

## 🔑 핵심 전략 정의

### NET 1위 — R6: 시초 갭 제한
```
[진입]
  s5 ≥ 7.0, a5 ≥ 5.0, score ≥ 30.0
  전일대비 ∈ [2.5%, 10%]
  offset ∈ [10, 90]분
  vwap_dist ≥ 1.0%, s3-s10 ≥ 1.0
  lookback_high ≥ 15봉, vol_ratio ≥ 3.0배
  ★ 시초 갭 (시가/전일종가 - 1) × 100 ≤ 3.0%   ← R6 신규
  진입 lag = +1 봉
[청산] SL_EOD, sl=-3.5%, target=없음
[운용] 하루 5종목, dedup
```

### WIN 1위 — R5: VWAP 연속 위 + 작은 익절
```
[진입]
  ... (위와 동일) ...
  ★ close > VWAP 연속 ≥ 3봉   ← R5 신규
[청산] TARGET_SL, sl=-3.5%, target=+2.0%   ← 작은 익절!
```

### 차등 NET 1위 — R59: R6 + COMBINED 가중치
```
가중치 함수: w = score × vol_ratio / max(0.5, ATR)
→ 신호 강도가 강하고 거래량 폭발 + 변동성 적정한 거래에 자본 집중
[주의] max pos 44% — 실전 적용 시 max_pos_cap=15% 강제 권장
```

---

## 💎 10가지 차등 투자 가중치 함수

| 코드 | 함수 | 의도 |
|---|---|---|
| `EQUAL` | w = 1 | 등액 (baseline) |
| `SCORE` | w = score | 신호 강도 비례 |
| `S5_A5` | w = s5 + a5 | VWAP 가속 강도 |
| `SCORE_SQ` | w = score² | 강한 신호 강조 |
| `INV_ATR` | w = 1 / ATR | 저변동성 우선 |
| `SCORE_INV_ATR` | w = score / ATR | Sharpe 형태 |
| `VWAP_DIST` | w = vwap_dist | VWAP 위 거리 비례 |
| `VOL_RATIO` | w = vol_ratio | 거래량 폭발 비례 |
| `COMBINED` | w = score × vol_ratio / max(0.5, ATR) | 다중 요인 통합 |
| `KELLY_PROXY` | score 단계별 0.1~1.0 | Kelly 분수 근사 |

---

## 📁 데이터·코드 위치 (개발자 메모)

원본 코드와 데이터는 별도 보관 (이 리포지터리에는 결과만 포함):

```
F:/_Project/                                    # 메인 워크스페이스
├─ _evwap_s_optimizer.py                       # 1차 최적화 엔진
├─ _evwap_s_round_optimizer.py                 # 10 라운드 엔진
├─ _evwap_s_100round_optimizer.py              # 100 라운드 + 차등투자 엔진
├─ _evwap_s_baseline_repro.py                  # baseline 재현
├─ _evwap_s_compare_baseline.py                # 비교
├─ _evwap_s_top_winrate.py                     # 승률 후보 평가
├─ _evwap_s_winrate_report.py                  # WIN 챔피언 리포트
├─ _evwap_s_round_report.py                    # 10R 리포트
├─ _evwap_s_final_consolidated_report.py       # 통합 리포트
├─ config/evwap_s_search_space.yaml            # 탐색 공간
├─ docs/plans/E_VWAP_S_OPTIMIZATION_PLAN.md    # 작업 계획서
└─ reports/evwap_s_optimization/
   ├─ cache.pkl (201MB)                        # 사전 계산 캐시
   ├─ all_trials.csv (6,900 trials)
   ├─ rounds_all_trials.csv (10 round)
   ├─ rounds100_all_trials.csv (100 round)
   ├─ baseline_result.{csv,md}
   ├─ top20_conditions.csv
   ├─ rounds_best.json / rounds100_best.json
   ├─ progress_log.md / rounds_progress.md / rounds100_progress.md
   ├─ comparison.json
   └─ final_recommendation.md

F:/KiwoomData/                                  # 1m bar CSV data (별도)
```

---

## ⚠️ 한계와 주의

1. **40 unique 거래일** — train 23 / val 8 / holdout 8. 통계적 표본 부족.
2. **Holdout 8일은 paper trading 1~2개월으로 보강 필수**.
3. **차등 투자 가중 NET +9.77%** 는 max pos 44% — 실전 적용 시 cap 강제.
4. **수수료 가정**: 0.83% (수수료 0.23 + 슬리피지 0.6). 거래세 0.20% 추가 시 NET 1.03% 차감.
5. **체결강도/외인·기관 데이터 미통합** — 추가 개선 여지.

---

**작성**: 2026-05-08 (Asia/Seoul)
**탐색**: 110 라운드 / 17,000+ trials 누적
**환경**: Python 3.13 (pandas 2.3, numpy 2.4) on Windows 11
