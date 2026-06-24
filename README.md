<div align="center">

# 🧠 QuantAI Platform

### AI-Powered Quantitative Trading Intelligence Platform

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://python.org)
[![React 18](https://img.shields.io/badge/react-18-61dafb.svg)](https://react.dev)
[![FastAPI](https://img.shields.io/badge/fastapi-0.111-009688.svg)](https://fastapi.tiangolo.com)
[![PyTorch](https://img.shields.io/badge/pytorch-2.3-ee4c2c.svg)](https://pytorch.org)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![MLflow](https://img.shields.io/badge/mlflow-2.13-0194E2.svg)](https://mlflow.org)
[![Docker](https://img.shields.io/badge/docker-ready-2496ED.svg)](docker-compose.yml)
[![Code Style: Black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

**Bloomberg Terminal + ChatGPT + Quant Research Platform — all in one**

[Live Demo](https://quantai.io) · [API Docs](http://localhost:8000/api/docs) · [Research Notes](research/papers/RESEARCH_NOTES.md) · [Architecture](docs/architecture/ARCHITECTURE.md)

---

</div>

## 🎯 What Is This?

QuantAI is a **production-grade, research-level quantitative trading intelligence platform** designed for daily use by quantitative analysts, algorithmic traders, and ML researchers.

It combines:
- **Deep learning** (LSTM · BiLSTM · GRU · Transformer · TFT · Informer)
- **Classical quant** (Markowitz · Black-Litterman · Risk Parity · HRP)
- **NLP** (FinBERT · VADER · TextBlob sentiment fusion)
- **Market microstructure** (regime detection, volatility modeling, factor analysis)
- **AI assistant** (QuantGPT for natural language market analysis)

> This is not a toy. Every component is production-ready with proper error handling, logging, testing, MLOps, and deployment infrastructure.

---

## ✨ Feature Highlights

### 🤖 AI / Deep Learning
| Feature | Details |
|---------|---------|
| Multi-horizon prediction | 1D · 3D · 7D · 14D · 30D forecasts |
| 9-model stacking ensemble | LSTM + BiLSTM + GRU + Transformer + Informer + TFT + XGBoost + LightGBM + CatBoost |
| Uncertainty quantification | MC Dropout (50 samples) + Quantile regression + Conformal prediction |
| Explainable AI | SHAP (DeepSHAP + TreeSHAP) + LIME local explanations |
| QuantGPT assistant | Natural language market analysis powered by LLM |
| Sentiment fusion | FinBERT + VADER + TextBlob → ensemble sentiment score |

### 📊 Quantitative Finance
| Feature | Details |
|---------|---------|
| 150+ engineered features | Technical · Statistical · Volatility · Volume · Pattern · Advanced math |
| Portfolio optimization | Mean-Variance · Black-Litterman · Risk Parity · HRP |
| Risk metrics | Sharpe · Sortino · Calmar · VaR · CVaR · Max Drawdown · Beta |
| Market regime detection | 6-state HMM + GMM clustering (bull/bear/sideways × low/high vol) |
| Backtesting engine | Vectorized + Event-driven with walk-forward validation |
| Advanced math features | Hurst exponent · Sample entropy · Fractal dimension · Garman-Klass vol |

### 🏗️ Platform Engineering
| Feature | Details |
|---------|---------|
| Production API | FastAPI + async SQLAlchemy + PostgreSQL + Redis caching |
| Modern frontend | React 18 + TypeScript + Tailwind + Recharts + D3 |
| MLOps | MLflow experiment tracking + Optuna HPO + DVC data versioning |
| Security | JWT auth + RBAC + rate limiting + input validation |
| CI/CD | GitHub Actions: test → lint → docker build → deploy |
| Containerization | Docker Compose (local) + Kubernetes manifests (cloud) |
| Task queue | Celery + Redis for async retraining and data ingestion |

---

## 🚀 Quick Start

### Option A: Docker (2 minutes)
```bash
git clone https://github.com/your-username/quantai-platform.git
cd quantai-platform
cp .env.example .env    # Add your API keys
docker-compose up -d
```
Open: **http://localhost:3000**

### Option B: Manual Setup
```bash
# 1. Backend
cd backend
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
cp ../.env.example .env

# 2. Database
docker-compose up postgres redis -d
python ../scripts/setup_db.py
python ../scripts/seed_data.py

# 3. Start backend
uvicorn app.main:app --reload --port 8000

# 4. Frontend (new terminal)
cd ../frontend
npm install && npm run dev
```

### Option C: Train Models
```bash
# Train LSTM for Apple
python scripts/retrain_models.py --ticker AAPL --model lstm

# Train ensemble for all default tickers
python scripts/retrain_models.py --ticker ALL --model ensemble

# Monitor in MLflow
open http://localhost:5000
```

---

## 📡 API Reference

Full interactive docs at `http://localhost:8000/api/docs`

### Key Endpoints

```http
POST /api/v1/predictions/predict
Content-Type: application/json

{
  "ticker": "AAPL",
  "horizons": [1, 3, 7, 14, 30],
  "model": "ensemble",
  "include_shap": true
}

# Response:
{
  "ticker": "AAPL",
  "currentPrice": 185.92,
  "horizons": [
    {
      "horizonDays": 7,
      "predictedPrice": 191.68,
      "predictedReturn": 0.031,
      "direction": "UP",
      "directionProbability": 0.64,
      "confidenceScore": 0.67,
      "uncertaintyLower": -0.045,
      "uncertaintyUpper": 0.092
    }
  ],
  "topFeatures": [...],
  "marketRegime": "bull_trending"
}
```

```http
POST /api/v1/portfolio/optimize
{
  "tickers": ["AAPL","MSFT","NVDA","GOOGL"],
  "method": "black_litterman",
  "max_weight": 0.40
}
```

```http
GET /api/v1/market/quote/NVDA
GET /api/v1/predictions/accuracy/TSLA
GET /api/v1/portfolio/risk-metrics/{portfolio_id}
```

---

## 🗂️ Dataset Sources

| Dataset | Source | Size | Purpose | Free? |
|---------|--------|------|---------|-------|
| Historical OHLCV | Yahoo Finance (yfinance) | Unlimited | Price prediction | ✅ Free |
| Fundamental data | Alpha Vantage | 25/day | Macro features | ✅ Free |
| Real-time quotes | Polygon.io | 5/min | Live data | ✅ Free |
| Financial news | NewsAPI | 100/day | Sentiment | ✅ Free |
| Macro indicators | FRED (St. Louis Fed) | Unlimited | GDP, rates, CPI | ✅ Free |
| Reddit sentiment | pushshift.io / PRAW | Variable | Social sentiment | ✅ Free |
| FinBERT model | HuggingFace Hub | 440 MB | NLP backbone | ✅ Free |
| SEC filings | SEC EDGAR API | Unlimited | Fundamental analysis | ✅ Free |

---

## 🏛️ Architecture

```
React Frontend (Port 3000)
    │
    │ HTTPS
    ▼
FastAPI Backend (Port 8000)
    ├── Predictions API  → ML Ensemble (LSTM+TFT+XGB...)
    ├── Portfolio API    → Optimizer (MV/BL/RP/HRP)
    ├── Market Data API  → Yahoo Finance / Alpha Vantage
    ├── Sentiment API    → FinBERT + NewsAPI
    └── AI Assistant     → QuantGPT (OpenAI/Anthropic)
          │
          ▼
    PostgreSQL (Data) + Redis (Cache) + Celery (Tasks)
          │
          ▼
    MLflow (Experiments) + DVC (Data Versioning)
```

See [full architecture diagram](docs/architecture/ARCHITECTURE.md).

---

## 🔬 Research Contributions

QuantAI implements several research-level techniques:

- **Temporal Fusion Transformer** — Full PyTorch implementation of Lim et al. 2021 with interpretable attention weights
- **PatchTST-style Transformer** — Patch-based tokenization for efficient long-horizon forecasting
- **Informer** — ProbSparse attention (O(L log L)) from Zhou et al. 2021
- **Hurst Exponent** — Rolling estimation of long-range dependence for regime awareness
- **Sample Entropy** — Complexity measure for confidence calibration
- **Garman-Klass Volatility** — OHLC-efficient estimator (5× more efficient than close-to-close)
- **HMM Regime Detection** — 6-state Gaussian HMM with transition probability tracking
- **Hierarchical Risk Parity** — de Prado (2016) clustering-based allocation

---

## 🛣️ Roadmap

- [ ] **v1.1** — Real-time WebSocket price streaming
- [ ] **v1.2** — Options pricing (Black-Scholes, binomial tree)
- [ ] **v1.3** — Reinforcement Learning trading agent (PPO/SAC)
- [ ] **v1.4** — Graph Neural Network for cross-asset correlation
- [ ] **v1.5** — Alternative data sources (satellite, credit card flow)
- [ ] **v2.0** — Multi-asset, multi-currency portfolio support
- [ ] **v2.1** — TimesFM / Chronos foundation model integration
- [ ] **v2.2** — Agentic AI workflows (AutoGen / LangGraph)

---

## 🧪 Testing

```bash
# Backend unit + integration tests
cd backend && pytest tests/ -v --cov=app --cov-report=html

# Feature engineering tests (150+ features)
pytest tests/test_features.py -v

# Backtesting engine tests
pytest tests/test_backtester.py -v

# Frontend type checking
cd frontend && npm run type-check
```

---

## 🐳 Deployment

### Local (Docker Compose)
```bash
docker-compose up -d
```

### AWS (ECS + RDS + ElastiCache)
```bash
# Terraform (see deployment/terraform/)
cd deployment/terraform
terraform init && terraform plan && terraform apply
```

### Kubernetes
```bash
kubectl apply -f deployment/kubernetes/
```

---

## 📄 License

[MIT License](LICENSE) — free for personal and commercial use.

---

## 🤝 Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for:
- Development setup
- Coding standards
- How to add new models
- Research contribution process

---

<div align="center">

**Built with ❤️ for quantitative researchers and algorithmic traders**

*If this platform helps your research or trading, please give it a ⭐*

</div>
