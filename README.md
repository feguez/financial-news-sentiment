# Financial News Sentiment with FinBERT

**Exploring the relationship between NVIDIA financial-news sentiment and short-term stock returns using a domain-specific transformer model.**

## Overview

Financial markets generate large amounts of qualitative information that can be difficult to analyze alongside quantitative price data. This project examines whether sentiment extracted from NVIDIA-related news headlines shows a measurable relationship with daily stock returns.

Using pretrained FinBERT, I converted financial-news headlines into numerical sentiment scores, aggregated them by publication date, and compared them with NVIDIA (NVDA) stock returns during January–February 2026.

**Research question:** Is financial-news sentiment associated with NVIDIA's same-date or next-trading-session returns?

The objective is exploratory financial-text analysis, not stock-price prediction or trading-strategy development.

## Methodology

The analysis follows five main stages:

1. **Data collection:** Retrieved NVIDIA-related headlines through Google News and historical NVDA stock prices through Yahoo Finance.
2. **Data preparation:** Standardized article metadata, removed duplicate records, and selected headlines explicitly mentioning NVIDIA or NVDA.
3. **Sentiment classification:** Applied pretrained [`ProsusAI/finbert`](https://huggingface.co/ProsusAI/finbert) to generate positive, negative, and neutral probabilities.
4. **Sentiment aggregation:** Calculated headline sentiment as `P(positive) - P(negative)` and averaged scores by publication date.
5. **Market analysis:** Compared daily sentiment with adjusted stock returns using Pearson correlations, confidence intervals, and robustness checks.

### Dataset

| Metric | Count |
|---|---:|
| Raw news records | 1,528 |
| Cleaned articles | 1,526 |
| NVIDIA/NVDA headline matches | 1,498 |
| Publication dates in primary sample | 58 |
| NVDA trading sessions | 39 |

The analysis uses a frozen dataset to preserve consistency across executions.

## Results

### Sentiment and Stock Returns

| Comparison | Observations | Pearson r | p-value |
|---|---:|---:|---:|
| Same-date sentiment vs. return | 38 | +0.2283 | 0.1681 |
| Sentiment vs. next-session return | 37 | -0.0647 | 0.7035 |

The same-date relationship was weakly positive, while the next-session relationship showed little linear association.

Neither correlation was statistically significant at the 5% level. The results do not establish that FinBERT sentiment predicts NVIDIA's stock returns.

### Robustness Checks

Additional analyses examined the sensitivity of these findings:

- **Headline relevance:** Including all cleaned headlines produced nearly identical correlations.
- **Calendar alignment:** Assigning weekend and holiday news to the next trading session did not materially change the results.
- **Leave-one-out analysis:** Individual trading days influenced the estimated correlations, particularly for the next-session comparison.

These findings reinforce the importance of evaluating data coverage, temporal assumptions, and statistical stability when using NLP signals in financial analysis.

## Key Limitations

- **Historical news coverage:** Google News retrieval was inconsistent and does not provide an exhaustive article archive.
- **Publication timing:** Retrieved timestamps were often standardized, limiting reliable intraday alignment with market movements.
- **Entity-specific interpretation:** FinBERT identifies financial sentiment in text, which may not represent the direction of the news's impact on NVIDIA specifically.
- **Limited observation period:** The analysis covers only two months and 39 trading sessions.
- **Other market drivers:** Returns reflect factors beyond news sentiment, including earnings expectations, broader market conditions, and investor positioning.

## Technologies

Python · pandas · PyTorch · Hugging Face Transformers · FinBERT · SciPy · Matplotlib · Google News · Yahoo Finance

## Project Files

| File | Description |
|---|---|
| [`financial_news_sentiment.ipynb`](financial_news_sentiment.ipynb) | Complete analysis, model inference, statistical tests, and visualizations |
| [`DATA.md`](DATA.md) | Data sources, preparation, limitations, and reproducibility |
| [`requirements.txt`](requirements.txt) | Python dependencies |

## Reproducibility

The notebook uses a frozen dataset archive (`finbert_canonical_v1.zip`) containing the collected news records, cleaned data, and historical stock returns.

The archive is not included in this public repository because it contains third-party news content. Full notebook execution therefore requires access to the separately maintained snapshot.

With the archive available:

1. Install dependencies using `pip install -r requirements.txt`.
2. Place `finbert_canonical_v1.zip` in the notebook's working directory.
3. Run the notebook from top to bottom.

The executed notebook preserves the analysis and results for review. See [`DATA.md`](DATA.md) for additional data documentation.

## Conclusion

FinBERT provides a practical way to transform unstructured financial headlines into measurable sentiment signals. However, in this sample, those signals did not show statistically significant relationships with NVIDIA's same-date or next-session returns.

The project demonstrates the value of combining transformer-based NLP with structured financial data, while emphasizing that interpretable AI outputs still require careful validation before they can inform decisions.