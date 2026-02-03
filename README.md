# Lightweight 24/7 Paper Trading Bot Specification

## Overview
This repository contains a complete technical specification for a **lightweight, server-side paper trading bot** designed for US Equities. It is optimized for "no-code" builders and engineers who want a robust, low-maintenance system to test trading strategies without risking real capital.

**⚠️ Disclaimer**: This system is for **educational and simulation purposes only**. It is designed to interact *only* with Paper Trading APIs (e.g., Alpaca Paper). Never connect this unmodified code to a real-money brokerage account. Past performance in backtests does not guarantee future results.

## Core Philosophy
- **Safety First**: strict separation from real-money endpoints.
- **Determinism**: No AI/LLM "hallucinations" in the trading loop. Logic is hard-coded and rule-based.
- **Simplicity**: Runs on a single container/VPS using standard Python tools.
- **Observability**: "If it's not logged, it didn't happen."

## Contents
1. **[Architecture & Design](ARCHITECTURE.md)**: The system's core loops, data flow, and infrastructure.
2. **[Strategy Specification](STRATEGY_SPEC.md)**: A robust "Sector Rotation" momentum strategy baseline.
3. **[Implementation Guide](IMPLEMENTATION_GUIDE.md)**: Python libraries, folder structure, and deployment steps.

## Quick Start (Concept)
The goal is to deploy a container that runs 24/7, wakes up on a schedule (e.g., market open), fetches data, calculates positions, and syncs the paper account.

```bash
# Conceptual Usage
git clone https://github.com/xrettle/paper-trade-bot-spec
cd paper-trade-bot-spec
# Review the docs to build your bot!
```
