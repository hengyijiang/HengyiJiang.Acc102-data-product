# WRDS Dividend Momentum Project

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![WRDS](https://img.shields.io/badge/WRDS-CRSP%20%7C%20Compustat-orange.svg)](https://wrds-www.wharton.upenn.edu/)

---

## 1. Problem & User

A teaching-oriented quantitative research project that screens stocks combining **positive price momentum** and **relatively high dividend yield** for finance students and researchers learning WRDS/CRSP/Compustat data workflows.



## 2. Data

**Source**: Wharton Research Data Services (WRDS)  
**Access**: CRSP & Compustat via institutional subscription  
**Key Fields**:

| Database | Table | Fields Used |
|----------|-------|-------------|
| CRSP | `crsp.msf` | `permno`, `date`, `prc`, `ret` |
| CRSP | `crsp.msenames` | `permno`, `ticker`, `comnam` |
| Compustat | `comp.funda` | `gvkey`, `fyear`, `dvpsx_f`/`dvc`/`dvpa` |
| CCM | `crsp.ccmxpf_linktable` | `gvkey`, `permno`, `linkdt`, `linkenddt` |

---

## 3. Methods

**Main Python Steps**:

1. **Data Extraction**
   - Query CRSP monthly stock prices and returns
   - Query Compustat annual dividend data
   - Merge via CCM link table (GVKEY ↔ PERMNO)

2. **Signal Construction**
   - **Momentum**: 12-month cumulative returns (skipping most recent month optional)
   - **Dividend Yield**: Annual dividend per share / current price
   - Apply 6-month reporting lag to avoid look-ahead bias

3. **Screening & Ranking**
   - Filter: momentum > 0 AND dividend yield in top 30%
   - Score: weighted combination (50% momentum + 50% dividend)
   - Select: top 20 stocks per month

4. **Backtest**
   - Equal-weight portfolio
   - Next-month holding period
   - Calculate cumulative returns and performance metrics

5. **Output Generation**
   - Export CSV files (holdings, returns, performance)
   - Generate plots (cumulative returns, signal scatter)
   - Save run summary as JSON

---

## 4. Key Findings

- **Dual-signal approach**: Combining momentum (trend) with dividend yield (fundamental support) creates a "quality momentum" stock universe
- **Look-ahead bias prevention**: 6-month accounting lag ensures realistic historical simulation using only publicly available data
- **Cross-sectional ranking**: Normalizing signals monthly makes the strategy robust across different market regimes
- **Simple but effective**: Equal-weight next-month backtest provides clean, interpretable results suitable for teaching
- **Full pipeline**: Complete workflow from raw WRDS data to performance report in one notebook

---

## 5. How to Run

### Step 1: Install Dependencies

```bash
pip install pandas numpy matplotlib python-dotenv wrds
```

### 2. Create a `.env` file

```env
WRDS_USERNAME=your_wrds_username
```

### 3. Run the notebook

Open `WRDS_jupyter.ipynb` and run the cells in order.

You can also override parameters inside the notebook:

```python
notebook_args = {
    "start_date": "2018-01-01",
    "end_date": "2024-12-31",
    "top_n": 10,
    "out_dir": "output_teaching"
}

main(notebook_overrides=notebook_args)
```
---
## 6.Product Link 



## 7. Limitations 

- This is a **teaching-oriented** implementation, not a production trading system.
- The backtest is intentionally simple: **monthly selection + next-month equal-weight return**.
- It does **not** model transaction costs, liquidity constraints, benchmark comparison, or advanced risk controls.
- Dividend field handling is practical and robust for coursework, but may need refinement for formal academic research.

## 8. Next Steps

- benchmark comparison
- transaction cost modeling
- long-short portfolio construction
- sector-neutral ranking
- additional factors such as value or profitability
- refactoring the notebook into a modular Python package

---

## 9.License

For learning, teaching, and research use
