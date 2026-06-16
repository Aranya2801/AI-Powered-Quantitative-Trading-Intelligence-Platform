# Contributing to QuantAI Platform

Thank you for your interest in contributing! QuantAI is a research-grade quantitative trading intelligence platform — we hold contributions to a high standard.

---

## Table of Contents
- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Architecture Guidelines](#architecture-guidelines)
- [Testing Requirements](#testing-requirements)
- [Pull Request Process](#pull-request-process)
- [Research Contributions](#research-contributions)

---

## Code of Conduct

This project adheres to professional standards expected in quantitative finance and ML engineering. Be respectful, constructive, and precise.

---

## Getting Started

### Prerequisites
- Python 3.11+
- Node.js 20+
- Docker & Docker Compose
- PostgreSQL 16+
- Redis 7+

### Fork & Clone
```bash
git clone https://github.com/YOUR_USERNAME/quantai-platform.git
cd quantai-platform
git remote add upstream https://github.com/quantai/quantai-platform.git
```

### Local Setup
```bash
# Backend
cd backend
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
cp ../.env.example .env  # fill in your keys

# Frontend
cd ../frontend
npm install

# Services
docker-compose up postgres redis -d
```

---

## Development Workflow

### Branch Naming
| Type | Pattern | Example |
|------|---------|---------|
| Feature | `feat/description` | `feat/add-catboost-ensemble` |
| Bug fix | `fix/description` | `fix/lstm-nan-loss` |
| Research | `research/description` | `research/graph-neural-network` |
| Docs | `docs/description` | `docs/backtesting-guide` |

### Commit Convention (Conventional Commits)
```
feat(ml): add Temporal Fusion Transformer with interpretable attention
fix(api): resolve race condition in portfolio optimizer endpoint
docs(research): add Hurst exponent methodology notes
perf(features): vectorize rolling entropy computation 10x speedup
test(backtest): add walk-forward validation coverage
```

---

## Architecture Guidelines

### Backend (Python / FastAPI)
- **Async everywhere** — all I/O must be async (`async def`, `await`)
- **Type hints required** — full typing on all function signatures
- **Pydantic v2 schemas** — validate all request/response models
- **Service layer** — business logic in `app/services/`, not in endpoints
- **Repository pattern** — DB queries in service layer only

```python
# ✅ GOOD
async def get_prediction(ticker: str, db: AsyncSession) -> PredictionResponse:
    ...

# ❌ BAD — sync, no typing
def get_prediction(ticker, db):
    ...
```

### ML Models
- All models must implement `predict_with_uncertainty()` returning confidence intervals
- All models must be compatible with SHAP explainability
- Training runs must be logged to MLflow with: params, metrics, artifacts, model
- Walk-forward validation required for any time-series model PR
- Minimum test: directional accuracy > 52% on held-out data

### Frontend (React / TypeScript)
- **No `any` types** — strict TypeScript required
- **No inline styles** — Tailwind classes only
- **Component size** — max 200 lines per component, split if larger
- **Custom hooks** — data fetching logic belongs in `src/hooks/`
- **Zustand** — global state management, no prop drilling

---

## Testing Requirements

### Backend
- Minimum **80% code coverage** for new features
- Unit tests for all service methods
- Integration tests for all API endpoints
- Use `pytest-asyncio` for async tests

```bash
cd backend
pytest tests/ -v --cov=app --cov-fail-under=80
```

### ML Models
- Regression tests: MAE, RMSE, Directional Accuracy benchmarks
- Backtesting: must show positive Sharpe on at least one asset
- Calibration: PICP within 5% of nominal coverage

### Frontend
```bash
cd frontend
npm run type-check  # TypeScript strict check
npm run build       # Must build without errors
```

---

## Pull Request Process

1. **Open an issue first** for features, research proposals, or architectural changes
2. **Keep PRs focused** — one feature or fix per PR
3. **Fill the PR template** completely
4. **All CI checks must pass** before review
5. **Two approvals required** for merging to `main`
6. **Squash commits** before merging

### PR Template
```markdown
## Summary
Brief description of changes.

## Type
- [ ] Feature
- [ ] Bug fix
- [ ] Performance improvement
- [ ] Research addition
- [ ] Documentation

## Changes
- ...

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests pass
- [ ] ML benchmarks maintained

## ML Metrics (if applicable)
| Metric | Before | After |
|--------|--------|-------|
| MAE    | ...    | ...   |
| Sharpe | ...    | ...   |
```

---

## Research Contributions

We actively encourage research contributions. To propose a new model or technique:

1. Open a **Research Issue** with:
   - Paper reference (arXiv, journal)
   - Expected improvement over baseline
   - Computational budget estimate
   - Dataset requirements

2. Implement in `research/experiments/` first
3. Validate on held-out data (minimum 2 years)
4. Write up results in `research/papers/` (LaTeX or Markdown)
5. Open a PR with the production implementation

### Research Areas We're Actively Exploring
- Graph Neural Networks for inter-stock correlation modeling
- Reinforcement Learning trading agents (PPO, SAC)
- Multimodal learning (price + news + alternative data)
- Foundation Models for time series (TimesFM, Chronos)
- Diffusion models for scenario generation
- Knowledge graphs for financial entity relationships

---

## Questions?

Open a [Discussion](https://github.com/quantai/quantai-platform/discussions) — we respond within 48 hours.
