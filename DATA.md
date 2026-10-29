# Data Documentation

## Data Sources

This project combines financial-news headlines with NVIDIA (NVDA) stock-price data from January–February 2026.

### Financial News
- **Source:** Google News, accessed through the Python `gnews` library
- **Search query:** `NVIDIA`
- **Period:** January 1 – February 28, 2026
- **Fields:** Headline, publication date, article URL, publisher, and description

### Stock Prices
- **Source:** Yahoo Finance, accessed through `yfinance`
- **Ticker:** NVDA
- **Period:** January–February 2026
- **Method:** Adjusted closing prices were used to calculate daily percentage returns. Additional December 2025 prices were retrieved to calculate the first January return.

## Collection and Preparation

Historical Google News retrieval produced inconsistent results across broad date ranges. The collection process was revised to use daily queries, with returned articles filtered to the intended publication date. Failed queries were retried.

The resulting dataset was cleaned by standardizing titles and publisher names, removing duplicate article records, and checking required fields.

| Dataset | Records |
|---|---:|
| Raw news records | 1,528 |
| Cleaned news articles | 1,526 |
| Headlines explicitly mentioning NVIDIA/NVDA | 1,498 |
| Primary-sample publication dates | 58 |
| Stock trading sessions | 39 |

The primary analysis uses headlines explicitly mentioning NVIDIA or NVDA. The full cleaned sample is retained for robustness comparisons.

## Sentiment Processing

News headlines were scored using the pretrained [`ProsusAI/finbert`](https://huggingface.co/ProsusAI/finbert) model from Hugging Face.

Headline sentiment was calculated as:

`P(positive) - P(negative)`

Daily sentiment was calculated as the arithmetic mean of available headline scores for each publication date.

News sentiment was then joined with stock returns using the trading calendar. Missing sentiment observations were not replaced with zero.

## Data Limitations

- **News coverage:** Google News results are not an exhaustive historical archive. Retrieval coverage and article selection may vary between executions.
- **Publication timing:** Most retrieved publication timestamps appeared standardized, preventing reliable intraday alignment with stock-market activity.
- **Repeated events:** Different articles may cover the same underlying event, even after exact-duplicate removal.
- **Sentiment interpretation:** A headline's financial sentiment may not represent its specific implications for NVIDIA.
- **Sample size:** The analysis covers a relatively short period and should not be interpreted as evidence of investment performance or forecasting ability.

## Reproducibility

The analysis uses a frozen dataset snapshot (`finbert_canonical_v1`) to avoid changes caused by repeated historical news searches.

The archive contains collected news records, cleaned and scored data, stock returns, and intermediate analytical datasets.

The frozen archive is maintained separately and is not included in the public repository because it contains third-party news content.

As a result, the public notebook documents the analytical workflow and saved results, but reproducing its execution requires access to the frozen data archive. The public repository does not currently provide a fully independent data-retrieval and reproduction pipeline.