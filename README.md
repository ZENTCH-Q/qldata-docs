<div align="center">

# qldata

**Production-ready Python library for cryptocurrency market data**

*Acquire, store, transform, and validate market data with a beautiful, Pythonic API*

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-0.3.0-green.svg)](https://github.com/ZENTCH-Q/qldata-docs)
[![Documentation](https://img.shields.io/badge/docs-qldata--docs-purple.svg)](https://zentch-q.github.io/qldata-docs/)

[Documentation](https://zentch-q.github.io/qldata-docs/) •
[Examples](https://zentch-q.github.io/qldata-docs/cookbook/) •
[Changelog](https://zentch-q.github.io/qldata-docs/changelog/)

</div>

---

## ✨ Features

<table>
<tr>
<td width="50%">

### 🎯 Fluent API Design
```python
import qldata as qd

# One-liner to get clean data
df = qd.data("BTCUSDT", source="binance")
    .last(30)
    .resolution("1h")
    .clean()
    .get()
```

</td>
<td width="50%">

### 📡 Real-Time Streaming
```python
# Live data with resilience built-in
stream = qd.stream(["BTCUSDT", "ETHUSDT"])
    .resolution("tick")
    .on_data(handle_tick)
    .get(start=True)
```

</td>
</tr>
<tr>
<td width="50%">

### 🔄 Data Transforms
```python
# Clean, fill, and resample
clean_df = (
    qd.data("BTCUSDT", source="binance")
    .last(7)
    .resolution("1m")
    .clean(remove_outliers=True)
    .fill_forward()
    .resample("1h")
    .get()
)
```

</td>
<td width="50%">

### 🛡️ Production-Ready
```python
# Built-in monitoring & alerts
from qldata.monitoring import (
    DataQualityMonitor,
    AlertManager
)

monitor = DataQualityMonitor()
alerts = AlertManager()
alerts.on_stale_data(send_alert)
```

</td>
</tr>
</table>

---

## 🚀 Quick Start

### Installation

```bash
# Full installation (recommended)
pip install qldata

# Minimal (core only, no broker dependencies)
pip install qldata[minimal]

# Specific exchanges
pip install qldata[binance]  # Binance only
pip install qldata[bybit]    # Bybit only
```

### Your First Query

```python
import qldata as qd

# Fetch the last 30 days of hourly BTC data from Binance
df = qd.data("BTCUSDT", source="binance", category="spot") \
    .last(30) \
    .resolution("1h") \
    .get()

print(df.head())
#                            open      high       low     close      volume
# timestamp                                                                 
# 2024-11-05 00:00:00  69500.00  69750.00  69400.00  69600.00  1250.5432
# 2024-11-05 01:00:00  69600.00  69800.00  69550.00  69750.00  1180.2341
# ...
```

### Live Streaming

```python
import qldata as qd

def handle_data(df):
    """Process incoming tick data."""
    if not df.empty:
        latest = df.iloc[-1]
        print(f"[{latest['symbol']}] Price: {latest['price']}")

# Start streaming with auto-reconnect
stream = qd.stream(["BTCUSDT", "ETHUSDT"], source="binance") \
    .resolution("tick") \
    .on_data(handle_data) \
    .get(start=True)

# Stream runs until you stop it
# stream.stop()
```

---

## 📦 Supported Exchanges

| Exchange | Spot | Perpetuals | Streaming | Status |
|----------|:----:|:----------:|:---------:|:------:|
| Binance  | ✅   | ✅ (USDM)  | ✅        | Stable |
| Bybit    | ✅   | ✅ (Linear)| ✅        | Stable |

---

## 🧰 Core Capabilities

### Historical Data
- **Fluent query builder** for intuitive data fetching
- **Multi-symbol parallel downloads** with configurable workers
- **Automatic pagination** for large date ranges
- **Built-in caching** for repeated queries

### Live Streaming
- **WebSocket connections** with auto-reconnect
- **Rate limit management** to respect exchange limits
- **Sequence tracking** to detect missed messages
- **Time synchronization** for accurate timestamps

### Data Quality
- **Adaptive cleaning** that detects data type (OHLCV, tick, etc.)
- **Outlier detection** using statistical methods
- **Gap analysis** to find missing data periods
- **Validation rules** for data integrity

### Monitoring & Resilience
- **Latency tracking** (P50, P95, P99)
- **Throughput monitoring** for data rates
- **Stale data detection** with configurable thresholds
- **Alert callbacks** for production systems

---

## 📚 Documentation

Comprehensive documentation is available at **[zentch-q.github.io/qldata-docs](https://zentch-q.github.io/qldata-docs/)**

- [📖 User Guide](https://zentch-q.github.io/qldata-docs/user-guide/installation/) - Installation, quick start, core concepts
- [🔧 API Reference](https://zentch-q.github.io/qldata-docs/api/historical-data/) - Detailed API documentation
- [📓 Cookbook](https://zentch-q.github.io/qldata-docs/cookbook/) - Real-world examples and recipes
- [📋 Changelog](https://zentch-q.github.io/qldata-docs/changelog/) - Version history and updates

---

## 🏗️ Architecture Overview

```
qldata
├── api/            # Unified API layer (qd.data, qd.stream)
├── adapters/       # Exchange-specific implementations
│   └── brokers/    # Binance, Bybit adapters
├── models/         # Data models (Bar, Tick, OrderBook, etc.)
├── transforms/     # Data cleaning and transformation
├── validation/     # Data quality checks and rules
├── resilience/     # Connection management, rate limiting
├── monitoring/     # Metrics, alerts, health checks
└── stores/         # Storage backends (Parquet, DuckDB)
```
## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**Made with ❤️ by [ZENTCH-Q](https://github.com/ZENTCH-Q)**

</div>

