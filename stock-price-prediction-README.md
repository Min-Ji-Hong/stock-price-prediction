# Stock Price Prediction with Cross-Firm Integration

> Can we capture "external market conditions" without expensive NLP pipelines — by modeling how shocks ripple across firms instead?

**Team:** Minseo Kim, **Jihong Min**, Dayoon Lee

📄 [Report / Slides](./Stock_Price_Prediction.pdf)

---

## Overview

Traditional price-only models fail to reflect external factors like news and
macroeconomic shocks. Most recent approaches try to capture this directly by processing
large volumes of financial text (NLP-based), which is costly and resource-intensive.

This project instead explores an **interconnectedness-based approach**: rather than
reading the news, we look at how a shock ripples through *related firms* — treating
cross-firm co-movement as an efficient proxy for external market conditions.

## My Role

- Exploratory data analysis and preprocessing for the Tesla case study (log transformation, missing-value interpolation, outlier detection/handling)
- Contributed to building and evaluating the baseline forecasting models (SES, SARIMA, LSTM)

<!-- 실제로 다른 파트(예: correlation/interconnectedness 분석, GDELT sentiment 파트 등)를 맡으셨다면 자유롭게 수정해주세요 -->

## Approach

| Track | Method |
|---|---|
| Sentiment-based (baseline) | GDELT Tone score from global news articles |
| Correlation / interconnectedness-based | Pearson correlation, Cross-Correlation Function (CCF), DTW similarity, Granger causality test → correlated features (log-return, volatility) → VARMA / LSTM |

## Data & EDA (Tesla case study)

- **Log transformation** applied to stabilize scale across the 2015–2026 price range
- **Missing values** (from aligning domestic/international company calendars) filled with 101 interpolated points, preserving the original trend
- **Outliers** (post rapid-increase prices, detected via 3σ) were **preserved** — they reflect genuine characteristics of Tesla's price behavior rather than noise

## Baseline Performance

| Model | MAE | RMSE | MAPE (%) |
|---|---|---|---|
| SES | 69.13 | 84.11 | 25.55 |
| SARIMA | 69.19 | 84.07 | 25.60 |
| **LSTM** | **22.62** | **30.10** | **7.63** |

LSTM captured variation relatively well but still showed a gap during sharp
external-event-driven moves — e.g. it couldn't reflect the Cybercab robotaxi
announcement (Oct 2024) since that shock isn't visible in a related firm's (Hyundai)
data, whereas broader EV market expansion news *did* show up as cross-firm signal.

## Repo Structure

```
├── docs/                # report / slides
├── notebooks/           # EDA, preprocessing, baseline models (SES/SARIMA/LSTM)
├── src/                 # (optional) reusable pipeline code
└── README.md
```

<!-- TODO: 실제 코드/노트북을 올린 뒤 위 구조를 실제 폴더 구성에 맞게 수정해주세요 -->

## Summary

1. Naive (price-only) models fail to capture variation driven by external events.
2. NLP-based approaches proposed in prior work are too costly to reproduce at scale.
3. Next step: evaluate whether the interconnectedness-based approach can match or beat
   the sentiment-based approach directly, rather than relying on price-only baselines.
