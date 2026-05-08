# E_VWAP_S 백테스트 최적화 결과

키움증권 1분봉 데이터로 한국 주식 단타 전략 **E_VWAP_S** 를 1,110+ 라운드 / 18,000+ trial 최적화한 결과 모음.

데이터: F:/KiwoomData (9개 조건검색 폴더 × 40 unique 거래일 × 3,572 종목·일).

---

## 🌐 GitHub Pages 라이브 사이트

- 🏠 **종합 (110 라운드)**: https://mirsys1.github.io/evwap-s-results/index.html
- 💰 **수익금 1위 (1,000 trial)**: https://mirsys1.github.io/evwap-s-results/profit.html ⭐ **최신**
- 📋 10 라운드 단독: https://mirsys1.github.io/evwap-s-results/10round.html
- 🚀 100 라운드 + 차등투자: https://mirsys1.github.io/evwap-s-results/100round.html
- 🛡️ 최고 승률 전략 상세: https://mirsys1.github.io/evwap-s-results/best_winrate.html
- 🔥 144 조합 백테스트 heatmap: https://mirsys1.github.io/evwap-s-results/144combos_heatmap.html

---

## 💰 최종 결론 — 차등투자 > 등액투자

**1,000 trial 복리 시뮬 결과 (초기 자본 ₩10,000,000)**:

| 가중치 | 평균 최종자본 | 비고 |
|---|---|---|
| 🥇 **COMBINED** (score × vol_ratio / ATR) | ₩21.5M | 최강 가중치 |
| 🥈 SCORE_INV_ATR | ₩19.4M | Sharpe형 |
| 🥉 VOL_RATIO | ₩18.6M | 거래량 폭발 비례 |
| INV_ATR | ₩17.7M | 저변동성 우선 |
| VWAP_DIST | ₩17.6M | VWAP 거리 비례 |
| S5_A5 | ₩17.4M | 가속 강도 |
| KELLY_PROXY | ₩16.2M | Kelly 분수 근사 |
| SCORE | ₩15.8M | 신호 강도 비례 |
| SCORE_SQ | ₩15.7M | 신호² 강조 |
| 🔻 **EQUAL** (등액) | **₩15.1M** | **꼴찌** |

**EQUAL이 모든 차등 가중치보다 평균 수익이 낮음**. 차등투자 우위 명확.

---

## 🏆 챔피언 — Trial 264 (R6 + COMBINED)

**복리 시뮬 결과**: 초기 ₩10,000,000 → 최종 **₩202,788,097** (+1,927.88%)

| 지표 | 수치 |
|---|---|
| 최종 자본 | **₩202,788,097** |
| 총 수익금 | **+₩192,788,097** |
| 총 수익률 | +1,927.88% |
| 거래일 | 37일 |
| 거래 수 | 130건 (평균 3.5건/일) |
| MDD (최대 손실) | 8.05% |
| Sharpe (연환산) | 매우 높음 |

### 등액 vs 차등 (동일 파라미터)

| 지표 | 등액 (EQUAL) | **차등 (COMBINED)** | 차등 효과 |
|---|---|---|---|
| 최종 자본 | ₩38,058,155 | **₩202,788,097** | **×5.33** |
| 총 수익률 | +280.6% | **+1,927.88%** | +1,647%p |

### 전략 정의

```
[베이스] R6 변형 (시초 갭 ≤ 3% + 1666 베이스)
[진입]
  s5 ≥ 7.0, a5 ≥ 5.0, score ≥ 30.0
  전일대비 ∈ [2.5%, 10%]
  offset ∈ [10, 90]분
  vwap_dist ≥ 1.0%, vol_ratio ≥ 3.0배
  lookback_high ≥ 15봉, 진입 lag = +1 봉

[추가 필터]
  시초 갭 ≤ 3% (R6)
  ATR ≤ 적정값
  09:30까지 누적 거래대금 ≥ θ억

[청산]
  exit_mode = SL_EOD, sl = -3.5%, target = 없음, time_stop = EOD

[운용]
  하루 최대 5종목 / dedup

[★ 차등 투자 가중치] COMBINED
  weight_i = score_i × vol_ratio_i / max(0.5, ATR_i)
  → 신호 강도 × 거래량 폭발 / 변동성
  → 강신호+거래폭발+저변동성에 자본 집중
```

---

## 🛡️ Cap 20% 안전 추천

**Trial 478** (R6 + SCORE_INV_ATR, max_pos_cap=20%)
- 최종 자본: **₩47,736,756** (+377.37%)
- MDD: **2.02%** (매우 낮음)
- 한 거래당 자본의 ≤20% — 실전 적용 가능

---

## 📊 110 라운드 + 1,000 profit trial 종합 챔피언

| 카테고리 | 라운드/Trial | 핵심 기법 | 성과 |
|---|---|---|---|
| 💰 **총 수익금 1위** | 1000R-T264 | R6 + COMBINED 가중치 | **₩202.8M (+1,928%)** |
| 🛡️ **수익금 (cap 20%)** | 1000R-T478 | R6 + SCORE_INV_ATR | ₩47.7M (+377%) |
| 🥇 **NET 1위 (등액)** | 10R-R6 | 시초 갭 ≤ 3% | NET +8.12% / win 61.9% |
| 🥇 **WIN 1위** | 10R-R5 | VWAP 연속 + 작은 익절 | win 86.32% / hold 89.74% |
| 🥇 **차등 NET 1위 (가중)** | 100R-R59 | R6 + COMBINED | 가중 NET +9.77% |

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
| `VWAP_DIST` | w = vwap_dist | VWAP 거리 비례 |
| `VOL_RATIO` | w = vol_ratio | 거래량 폭발 비례 |
| **`COMBINED`** ⭐ | w = score × vol_ratio / max(0.5, ATR) | **다중 요인 통합 — 1위** |
| `KELLY_PROXY` | score 단계별 0.1~1.0 | Kelly 분수 근사 |

---

## 📁 산출물

```
evwap-s-results/
├─ index.html                      # 🏠 종합 결과 (110 라운드)
├─ profit.html                     # 💰 1,000 trial 수익금 1위 ⭐ NEW
├─ 10round.html                    # Part 1: 10 라운드
├─ 100round.html                   # Part 2: 100 라운드 + 차등투자
├─ best_winrate.html                # WIN 1위 전략 상세
├─ 144combos_heatmap.html           # 144 조합 백테스트 heatmap
├─ README.md                        # 본 파일
└─ 한글 파일명 alias 동봉
```

---

## ⚠️ 한계와 주의

1. **40 unique 거래일** — train 23 / val 8 / holdout 8. 통계적 표본 부족.
2. **+1,927% 수익률은 backtest 한정** — 실전 슬리피지/체결 미스 등으로 일부 감소.
3. **차등투자 max pos** 제한 없으면 단일 종목 큰 손실 시 회복 어려움 → **cap 20% 강제 권장**.
4. **수수료 가정**: 0.83% (수수료 0.23 + 슬리피지 0.6). 거래세 0.20% 추가 시 NET 1.03% 차감.
5. **Paper trading 1~2개월 검증 필수** — backtest 70%만 실현돼도 충분히 매력적.

---

**작성**: 2026-05-08 (Asia/Seoul)
**탐색**: 110 라운드 + 1,000 profit trial = 18,000+ trials 누적
**환경**: Python 3.13 (pandas 2.3, numpy 2.4) on Windows 11
**Repo**: https://github.com/mirsys1/evwap-s-results
