# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **RD-Agent (Research & Development Agent)** (`rd-agent`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** RD-Agent (Research & Development Agent) (`rd-agent`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Data & Analytics / Autonomous Research & Development  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

RD-Agent is an autonomous research and development agent framework developed by Microsoft Research designed to automate industrial-grade data science, quantitative financial modeling, alpha factor discovery, and reinforcement learning research. Rather than treating machine learning as ad-hoc prompt engineering or isolated code completion, RD-Agent structures scientific discovery into an evolutionary loop: formulating theoretically sound hypotheses, implementing modular experimentation code, executing trials in resource-governed sandboxes, computing statistical performance metrics, and distilling empirical findings into an evolving research knowledge graph.

### 1. Decision Architecture

The research objective intake, hypothesis generation, code synthesis, sandboxed execution, and knowledge distillation pipeline operates across a deterministic, five-stage architecture:

```
Research Objective / Scientific Challenge (e.g., "Discover novel high-capacity alpha factors for equity returns")
    │
    ▼
[Stage 1: Domain Analysis & Prior Knowledge Retrieval]
    │  - Analyzes dataset schema, feature distributions, and baseline performance benchmarks
    │  - Queries evolving research knowledge graph to identify previously tested hypotheses
    │  - Prevents circular exploration by filtering out past failed or duplicate concepts
    ▼
[Stage 2: Autonomous Hypothesis & Mathematical Formulation]
    │  - Formulates novel research hypothesis backed by domain principles or financial theory
    │  - Drafts explicit mathematical transformations (e.g., cross-sectional rank, rolling decay)
    │  - Establishes falsifiable quantitative target criteria and baseline comparison thresholds
    ▼
[Stage 3: Vectorized Experiment Code Synthesis]
    │  - Synthesizes modular, fully typed Python code (Qlib factor processors, PyTorch Lightning)
    │  - Injects defensive safeguards against division-by-zero, lookahead bias, and numerical overflow
    │  - Performs static syntax and import checks before sandbox deployment
    ▼
[Stage 4: Sandboxed Execution & Telemetry Monitoring]
    │  - Executes training or backtesting scripts in isolated Docker/subprocess sandboxes
    │  - Enforces GPU/CPU resource caps, memory bounds, and execution timeouts (default 600s)
    │  - Streams stdout/stderr, loss convergence curves, and runtime diagnostic traces
    ▼
[Stage 5: Quantitative Metric Evaluation & Knowledge Graph Evolution]
    │  - Computes domain performance metrics (Information Coefficient, Sharpe ratio, RMSE, AUC-ROC)
    │  - Conducts statistical hypothesis tests against historical baselines to verify out-of-sample edge
    │  - Commits distilled insights and post-mortems to the research knowledge base to drive next cycle
    ▼
Verified Quantitative Model / Evolved Alpha Factor Registry & Auditable Experiment Trajectory
```

### 2. Decision Logic & Routing Formulations

RD-Agent evaluates hypothesis viability, alpha factor quality, and evolutionary progression using deterministic mathematical models:

1. **Hypothesis Prioritization Score ($S_{\text{hypo}}$)**:
   $$S_{\text{hypo}}(H) = (w_n \cdot N_{\text{novelty}}) + (w_t \cdot T_{\text{theoretical}}) + (w_f \cdot F_{\text{feasibility}})$$
   where:
   - $N_{\text{novelty}} \in [0, 1]$ measures semantic dissimilarity against past explored hypotheses in the knowledge graph.
   - $T_{\text{theoretical}} \in [0, 1]$ represents alignment with domain mechanics (market microstructure, econometric principles).
   - $F_{\text{feasibility}} \in \{0, 1\}$ verifies that required features and computational resources are available.
   - Weights: $w_n = 0.40, w_t = 0.40, w_f = 0.20$ ($\sum w_i = 1.0$).

2. **Quantitative Factor Edge ($\text{IC}_{\text{rank}}$)**:
   $$\text{IC}_{\text{rank}} = \frac{1}{T} \sum_{t=1}^{T} \rho\left(\text{rank}(f_t), \text{rank}(r_{t+1})\right)$$
   where $\rho$ represents Spearman rank correlation between predicted factor scores $f_t$ and forward returns $r_{t+1}$ across cross-sectional assets. A candidate alpha factor advances only when $\text{IC}_{\text{rank}} \ge 0.05$ and the annualized Information Ratio $\text{IR} \ge 1.5$.

### 3. Thresholding & Refusal Decision Criteria

RD-Agent enforces strict operational safeguards to protect system integrity and financial safety:
- **Refusal to Execute Code with Lookahead Bias**: Time-series experiments attempting to reference future timestamps or shuffle temporal ordering without lagging are rejected (`ERR_LOOKAHEAD_BIAS_DETECTED`).
- **Refusal of Direct Live Market Deployment**: Autonomous deployment of trading strategies to live broker accounts or production market gateways is strictly forbidden (`ERR_LIVE_TRADING_DEPLOYMENT_PROHIBITED`).
- **Execution Timeout Ceilings**: Training jobs or backtests exceeding 600 seconds are terminated via SIGKILL to protect shared GPU clusters (`WARN_EXPERIMENT_TIMEOUT_EXCEEDED`).
- **Refusal to Skip Baseline Comparison**: Candidate models cannot be accepted without empirical verification against predefined baseline models (`ERR_BASELINE_BENCHMARK_UNSATISFIED`).

### 4. Fallback Decision Mechanism

Continuous research pipeline stability is maintained through multi-tier fault recovery:
- **Model Provider Cascade**: If foundation LLM inference endpoints experience rate limits (HTTP 429) or timeouts, the orchestrator cascades across Azure OpenAI, Anthropic, and local open-weight endpoints.
- **Automated Numerical Fallback**: If factor generation encounters division-by-zero or non-finite values (NaNs), automated cleaning wrappers apply cross-sectional median imputation and winsorization.
- **Evolutionary Backtracking**: If three consecutive experimental iterations fail to outperform the benchmark, the orchestrator backtracks to the highest-scoring parent node in the research knowledge graph.

### 5. Human-in-the-Loop Governance

Human researchers maintain ultimate scientific oversight and operational authority:
- **Interactive Streamlit Web Dashboard**: Researchers can inspect loss curves, factor distributions, backtest tearsheets, and agent reasoning traces in real-time.
- **Hypothesis Injection & Steering**: Humans can inject custom hypotheses, specify mandatory constraint sets, or veto proposed model architectures.
- **Structured Audit Trails**: Every hypothesis proposal, Python script version, execution log, and metric evaluation is persisted in append-only storage for full scientific reproducibility.

---

## The Data It Uses

RD-Agent operates under strict data integrity, mathematical rigor, and research privacy standards.

### 1. Ingested Input Data

The agent processes only operational assets necessary to conduct data science and research experiments:
- **Research Directives**: Natural language goals, problem specifications, and target evaluation metrics.
- **Scientific & Tabular Datasets**: Numerical feature matrices, market order book data, tabular CSVs, and benchmark datasets.
- **Execution Feedback Streams**: PyTorch training logs, stdout/stderr streams, loss curves, and backtest performance summaries.

### 2. Configuration & Reference Data

- **Scenario Configs**: Declarative YAML configurations defining datasets, backtest horizons, and modeling constraints.
- **Research Knowledge Graph**: Historical database of proposed hypotheses, factor formulations, and empirical outcome metrics.
- **Domain Ontologies**: Mathematical formula libraries and quantitative operator registries.

### 3. Base Model & Inference Lineage

- **Deterministic Evaluation Engine**: Python numerical libraries (NumPy, Pandas, SciPy, Qlib), PyTorch backends, and metric calculators execute with 100% mathematical determinism.
- **Foundation LLMs**: High-capability reasoning models (GPT-4o, Claude 3.5 Sonnet, DeepSeek-Coder) utilized for hypothesis ideation, mathematical factor formulation, and script authoring.
- **Zero Training on User Datasets**: Proprietary financial datasets, corporate feature sets, and internal alpha formulas are never utilized for external model retraining.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Hardened**: Defended against prompt injection in untrusted datasets, code exfiltration, and supply-chain package tampering.
- **Local Sandbox Storage**: Experimental code, intermediate weights, and metric artifacts reside strictly in local workspace directories or private container volumes.
- **Automated Credential Redaction**: API keys, database connection strings, and internal server endpoints are scrubbed from exported traces.
- **Zero Commercial Monetization**: User research data, proprietary trading signals, and machine learning models are never monetized or shared with third parties.

---

## Limitations

Understanding the operational boundaries and technical constraints of RD-Agent is essential for effective deployment.

### 1. High Compute and GPU Demands
- **Limitation**: Deep neural architecture search and large-scale backtesting can incur heavy GPU memory and compute consumption.
- **Mitigation**: RD-Agent provides sub-sampling heuristics, early-stopping triggers, and lightweight gradient-boosted tree baselines.

### 2. Financial Regime Shift and Non-Stationarity
- **Limitation**: Factors and models optimized on historical backtest data may experience alpha decay during macroeconomic regime shifts.
- **Mitigation**: RD-Agent enforces rolling walk-forward cross-validation and evaluates factors across multiple distinct market regimes.

### 3. Unobserved Market Microstructure Friction
- **Limitation**: Backtests in quantitative finance may underestimate real-world market impact, slippage, and liquidity constraints.
- **Mitigation**: Simulation scenarios incorporate realistic fee models, turnover penalties, and volume-weighted execution assumptions.

### 4. Mathematical Reasoning Hallucinations
- **Limitation**: Generative LLMs may occasionally propose factor formulas that are mathematically degenerate or produce degenerate constant matrices.
- **Mitigation**: Deterministic symbolic math checkers and rank correlation assertions validate factor outputs prior to execution.

### 5. Multi-Gigabyte Raw Dataset Handling
- **Limitation**: Datasets exceeding local RAM limits can cause out-of-memory exceptions during single-threaded processing.
- **Mitigation**: The framework integrates Polars, chunked Parquet readers, and memory-mapped arrays for large-scale data processing.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & routing formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested research directives, datasets & execution streams | Section 1 | Verified |
| - Configuration, scenario configs & research knowledge graph | Section 2 | Verified |
| - Base model lineage & deterministic evaluation engine | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - High compute and GPU demands | Section 1 | Verified |
| - Financial regime shift and non-stationarity | Section 2 | Verified |
| - Unobserved market microstructure friction | Section 3 | Verified |
| - Mathematical reasoning hallucinations | Section 4 | Verified |
| - Multi-gigabyte raw dataset handling | Section 5 | Verified |
