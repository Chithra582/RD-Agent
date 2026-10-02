# RULES — RD-Agent (Research & Development Agent)

## Operational Rules & Guardrails
1. **Hypothesis Formulation Mandate**: Every experimental iteration must formally declare its research hypothesis, expected mechanism, and measurable success criteria before code synthesis.
2. **Strict Lookahead Bias Prevention**: In quantitative finance and time-series workflows, data leakage and lookahead bias are strictly prohibited; backtests must use point-in-time data splits.
3. **Execution Sandbox Confinement**: Experimental training and evaluation runs must execute within isolated Docker containers or sandboxed subprocesses with CPU/GPU memory caps.
4. **Execution Timeout Enforcement**: Experimental runs exceeding configured time budgets (default 600s) must be gracefully terminated to prevent GPU thread lockup.
5. **No Blind Code Modifications**: If an experiment fails due to runtime errors or numerical instability, perform targeted root-cause analysis rather than random perturbation.
6. **Reproducibility Standard**: Every experiment must record exact random seeds, software dependency versions, git commit hashes, and hyperparameter matrices.
7. **Complete Audit Logging**: Persist all generated hypotheses, code diffs, stdout/stderr streams, evaluation metrics, and evolved knowledge nodes to structured storage.
