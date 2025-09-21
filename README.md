# 10-K Financial Chatbot Prototype
This repository provides a compact, learning-focused workflow that analyzes SEC 10-K data for Microsoft, Tesla, and Apple and powers a simplified chatbot inside a Jupyter Notebook to answer predefined financial queries. The repo contains only two files: a CSV with key metrics and a notebook that loads the CSV, computes growth/ratios, and responds to common questions.

## Files
- **10K_key_financials_MSFT_TSLA_AAPL.csv** — Extracted figures (USD millions): Total Revenue, Net Income, Total Assets, Total Liabilities, Cash Flow from Operating Activities (CFO).
- **chatbot_BCG.ipynb** — Jupyter Notebook that loads the CSV, computes YoY growth for Revenue/Net Income/CFO, leverage (Liabilities/Assets), cash conversion (CFO/Net Income), and answers predefined finance queries via a tiny rule-based chatbot.

## Install & Run
```bash
pip install pandas notebook
jupyter notebook
```
Open **chatbot_BCG.ipynb**, run all cells, then use the interactive cell to ask questions.

## Supported Queries
- What is the latest total revenue by company?
- How has net income changed over the last year?
- Which company has the strongest cash conversion?
- What is the leverage ratio last year?
- What is the 3-year revenue sum by company?
- Show summary

## Example
```
Q: Show summary
A:
Summary (YoY, latest FY):
- Microsoft: Revenue +15.7%, Net Income +21.8%, CFO +35.4%
- Tesla: Revenue +1.0%, Net Income -52.7%, CFO +12.6%
- Apple: N/A (fill from 10-K)
```

## Notebook Path Configuration
If using a fixed path (example macOS):
```python
from pathlib import Path
CSV_PATH = Path("/Users/aish/10K_key_financials_MSFT_TSLA_AAPL.csv")
```
Or place the CSV next to the notebook:
```python
from pathlib import Path
CSV_PATH = Path("10K_key_financials_MSFT_TSLA_AAPL.csv")
```

## How It Works
**Data:** Extracted from 10-Ks (Income Statement: Revenue, Net Income; Balance Sheet: Total Assets, Total Liabilities; Cash Flow: CFO). Units: USD millions. Fiscal year ends differ (MSFT: June, TSLA: Dec, AAPL: late Sept).  
**Cleaning:** Numeric coercion; sort by Company/Fiscal Year.  
**Derived metrics:**  
- YoY growth via `groupby` + `pct_change` for Revenue, Net Income, CFO.  
- Leverage = Total Liabilities / Total Assets.  
- Cash Conversion = CFO / Net Income.  
**Chatbot:** Maps predefined questions to answer functions reading latest-year rows and derived metrics.

## Tips & Troubleshooting
- **KeyError `'... Growth (%)' not in index'`**: Ensure YoY columns exist or use the growth-column mapping in the notebook.  
- **TypeError `tuple indices must be integers`**: Use `iterrows()` (applied in provided functions).  
- **Apple shows `N/A`**: Fill missing Apple values from the 10-K and re-run.  
- **Different CSV path**: Update `CSV_PATH`.

## Data Notes & Assumptions
- Balance sheets show two years per filing; older totals may require the prior year’s 10-K.  
- “Liabilities” and “Assets” are reported totals; “CFO” is net cash provided by operating activities.  
- Assumes consistent units (millions).

## Limitations
- Predefined queries only (no free-form NLP).  
- Missing CSV values return `N/A`.  
- Prototype for learning—not production.

## Learning Goals
- Extract and normalize financial statement data.  
- Use pandas to compute growth and ratios.  
- Build a minimal chatbot interface in Jupyter.

## Optional Enhancements
- Add matplotlib charts for Revenue/Net Income/CFO trends.  
- Alerts for large YoY changes (e.g., ±10%).  
- Query: “Which company reduced leverage most YoY?”  
- Wrap logic in Flask for a web demo.
