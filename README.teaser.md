# NullSpectreOS

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/)
[![Status](https://img.shields.io/badge/status-dropping%20soon-blueviolet.svg)](#)
[![Tests](https://img.shields.io/badge/tests-4752%20passing-brightgreen.svg)](#)
[![LOC](https://img.shields.io/badge/LOC-164%2C000%2B-informational.svg)](#)

**Institutional-grade quant trading OS. Zero mandatory dependencies. Built by a human and two AIs.**

---

## What this is

NullSpectreOS is a full-stack algorithmic trading platform built around a 9-agent governor pipeline. It handles market data ingestion, signal generation, risk enforcement, order execution, and post-trade attribution — all from a single Python package with **no mandatory third-party runtime dependencies**.

Paper trading, backtesting, research, and governance run completely offline on a clean Python 3.10+ install. No NumPy. No Pandas. No install ceremony.

This is not a framework you extend. This is an operating system for trading.

---

## Why it exists

Most serious quant tools are either:
- Closed-source institutional infrastructure you'll never touch
- Open-source libraries that give you building blocks but no system

NullSpectreOS is the third thing: a complete, opinionated, production-oriented trading OS that you own entirely — code, data, runtime, governance.

Inspired by Jigsaw Daytradr, InSilico Terminal, Quantpedia, and the workflows of prop-desk operators. Designed for a single operator running on a laptop or a $5 VPS.

---

## How it was built

This was built in a single sustained sprint by one human working alongside GPT and Claude as genuine coding partners — not autocomplete, but actual architectural collaboration across hundreds of sessions.

The result:

| Metric | Value |
|---|---|
| Source files | 281 Python modules |
| Test files | 284 test modules |
| Total LOC | ~164,000 |
| Test definitions | 4,752 passing |
| SQLite tables | 60+ in QuantOSRegistry |
| Zero mandatory runtime deps | ✓ |

Every number above is verifiable. The code drops in days.

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        NullSpectreOS                            │
│                    9-Agent Governor Pipeline                     │
├────────────────────────┬────────────────────────────────────────┤
│   DATA PLANE           │   SIGNAL PLANE                         │
│                        │                                        │
│  StreamManager ────────┼──► LiveIndicatorEngine                 │
│  (WebSocket feeds)     │         │                              │
│                        │         ▼                              │
│  MultiDB Loader ───────┼──► SignalEngine ──────────────────┐   │
│  (SQLite/DuckDB/PG)    │         │                         │   │
│                        │         ▼                         │   │
│  OrderFlowEngine ──────┼──► HypothesisAgent                │   │
│  (DOM, tape, flow)     │         │                         │   │
│                        │         ▼                         │   │
├────────────────────────┤   ValidationAgent                  │   │
│   GOVERNANCE PLANE     │         │                         │   │
│                        │         ▼                         │   │
│  GovernanceStack       │   GovernorPolicyAgent ◄───────────┘   │
│  ├─ LiveAuthEnforcer   │         │                             │
│  ├─ SecurityManager    │         ▼                             │
│  ├─ BootRecovery       │   PortfolioRiskAgent                  │
│  └─ AuditTrail         │         │                             │
│                        │         ▼                             │
├────────────────────────┤   ExecutionAgent                      │
│   EXECUTION PLANE      │         │                             │
│                        │    ┌────┴────┐                        │
│  ExecutionRouter ◄─────┼────┤  Paper  │  Binance (live)        │
│                        │    └─────────┘                        │
├────────────────────────┴────────────────────────────────────────┤
│   OBSERVABILITY                                                  │
│   NsosServer (REST + SSE) ── QuantOSRegistry (60+ SQLite tables)│
│   PostTradeAttributionAgent ── PySide6 Desktop Shell            │
└──────────────────────────────────────────────────────────────────┘
```

---

## What ships at release

- **9-agent governor pipeline** — Market Data, Data QA, Feature Engineering, Hypothesis, Validation, Governor Policy, Portfolio/Risk, Execution, Post-Trade Attribution
- **Paper and live trading** — full paper mode out of the box; Binance futures live execution available
- **Backtesting engine** — 150+ parameters, Monte Carlo, walk-forward, purged CV, advanced analytics
- **Portfolio simulation** — multi-symbol equity curves, VaR/CVaR, correlation, benchmark-relative analytics
- **PySide6 desktop shell** — native UI with real-time order book, DOM, governance monitoring, 6-desk layout
- **QuantOSRegistry workbench** — 60+ SQLite tables for runs, positions, incidents, risk snapshots
- **Multi-DB historical loader** — SQLite, DuckDB, Parquet, PostgreSQL, MySQL as first-class data sources
- **Governance stack** — LiveAuthEnforcer (BLOCK mode default), audit trails, BootRecovery, CrashRecovery
- **LLM task layer** — optional GPT/Claude/DeepSeek integration (never in the execution hot path)
- **API budgeting** — per-agent LLM spend caps, predictive cost forecasting
- **Widget marketplace** — extensible panel registry, plugin API for custom agents, indicators, data sources
- **stdlib-only core** — zero mandatory pip installs for paper mode

---

## Install (at release)

```bash
pip install -e .                      # paper trading, API server, full backtest engine
pip install -e ".[exchange]"          # + Binance live connectivity
pip install -e ".[db]"                # + DuckDB / PostgreSQL / MySQL loaders
pip install -e ".[llm]"               # + LLM task integrations
```

Everything runs on a Lenovo T490s. Everything runs on a $5 Hetzner VPS. No cloud lock-in.

---

## Security posture

- Auth defaults to `BLOCK` mode — startup refuses if checks fail in live mode
- `NSOS_SECRET_KEY` from env only — no secrets on disk
- Bootstrap key usage blocked by default in production
- Exchange API keys consumed from environment variables only, never written to disk
- Full audit trail on every governance decision

---

## Roadmap (post-release)

- `v0.2` — Live exchange foundation (Binance WebSocket, pre-trade risk gate, paper→live migration)
- `v0.3` — Multi-exchange (Bybit, OKX, unified order model)
- `v0.5` — Strategy framework (StrategySpec, versioning, walk-forward gate)
- `v0.9` — Ecosystem (plugin API, strategy portability, community infrastructure)
- `v1.0` — Production grade (multi-user, compliance export, Kubernetes)

---

## Watch for the drop

**Star and Watch this repo.** The full codebase — all 164,000 lines, all 284 test modules, all 60+ SQLite tables — drops in days. Not months. Days.

This will be MIT-licensed, no strings attached, no vendor lock-in, no telemetry.

If you've wanted a serious quant OS that you actually own, this is it.

---

> **Not financial advice.** NullSpectreOS is a software tool. All trading decisions and their consequences are solely the responsibility of the operator. Use at your own risk.
