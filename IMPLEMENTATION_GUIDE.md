# Implementation Guide

This guide describes how to implement the Paper Trading Bot using Python.

## 1. Project Structure
Create the following file structure:

```text
/paper-trade-bot
  ├── main.py              # Entry point + Scheduler
  ├── config.json          # User settings (Keys, Tickers)
  ├── requirements.txt     # Python dependencies
  ├── Dockerfile           # Deployment container
  ├── src/
  │   ├── __init__.py
  │   ├── broker.py        # Alpaca API Wrapper
  │   ├── strategy.py      # Momentum Logic
  │   └── data.py          # Data Fetching (yfinance/alpaca)
  └── logs/                # Local log storage
```

## 2. Dependencies (`requirements.txt`)
```text
alpaca-py==0.32.0   # Official SDK
pandas==2.2.0       # Dataframes
schedule==1.2.1     # Scheduler
python-dotenv==1.0.0 # Env vars
yfinance==0.2.36    # Backup data source
streamlit==1.31.0   # Dashboard (Optional)
```

## 3. Code Snippets

### A. Configuration (`config.json`)
```json
{
  "api_key": "PK****************",
  "api_secret": "******************",
  "base_url": "https://paper-api.alpaca.markets",
  "universe": ["XLK", "XLF", "XLE", "XLV", "XLI", "XLP", "XLY", "XLU", "XLB", "TLT"],
  "benchmark": "SPY",
  "lookback_days": 126,
  "top_n": 3
}
```

### B. The Scheduler (`main.py`)
```python
import schedule
import time
from src.strategy import run_bot_job

def job():
    print("Starting scheduled run...")
    try:
        run_bot_job()
    except Exception as e:
        print(f"CRITICAL ERROR: {e}")

# Schedule for 10:00 AM New York time (Market Open + 30m)
schedule.every().monday.at("10:00").do(job)

print("Bot is running...")
while True:
    schedule.run_pending()
    time.sleep(60)
```

### C. The Strategy Logic (`src/strategy.py`)
```python
import pandas as pd

def calculate_momentum(prices_df, lookback=126):
    """
    Returns a Series of momentum scores: (Price / Price_N_Ago) - 1
    """
    return (prices_df.iloc[-1] / prices_df.iloc[-lookback]) - 1

def select_portfolio(momentum_scores, top_n=3):
    """
    Returns list of symbols to buy.
    """
    return momentum_scores.sort_values(ascending=False).head(top_n).index.tolist()
```

## 4. Deployment (Docker)

**`Dockerfile`**:
```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "main.py"]
```

## 5. Next Steps for You
1.  **Get API Keys**: Sign up for Alpaca Paper Trading.
2.  **Clone this repo**: (Or create this structure locally).
3.  **Build**: `docker build -t paper-bot .`
4.  **Run**: `docker run -d --env-file .env paper-bot`
