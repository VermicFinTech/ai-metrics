# Analyzing AI Metrics

Analysis of anonymized, per-user monthly GitHub Copilot usage alongside pull request output (PRs, commits, additions/deletions), plus cost data. The work lives in a Jupyter notebook and uses pandas, matplotlib and plotly.

## Contents

| Path | Description |
|---|---|
| `analyze-metrics-1.ipynb` | Main analysis notebook (metric definitions, trends, statistics, adoption vs. output, usage tiers, treemaps, per-user views, Claude Code cost estimate) |
| `combined_report_full_anon.csv` | Current dataset, one row per `user_id` per `month` |
| `old data/` | Earlier snapshots of the dataset (kept for reference, not used by the notebook) |
| `main.py` | Placeholder entry point, not used |
| `pyproject.toml` / `uv.lock` | Dependencies, managed with [uv](https://docs.astral.sh/uv/) |

### Data columns

`user_id`, `month`, `cp_active_days`, `cp_interactions`, `cp_generations`, `cp_acceptances`, `cp_ai_loc_added`, `cp_ai_loc_deleted`, `prs_opened`, `prs_merged`, `pr_commits`, `pr_additions`, `pr_deletions`, `cp_ai_credits`, `cp_cost_usd`.

Full definitions are in the first cell of the notebook.

## Setup (uv + VS Code)

### 1. Prerequisites

- [uv](https://docs.astral.sh/uv/getting-started/installation/) (e.g. `brew install uv` on macOS)
- [Visual Studio Code](https://code.visualstudio.com/) with these extensions:
  - **Python** (`ms-python.python`)
  - **Jupyter** (`ms-toolsai.jupyter`)

uv installs the required Python version (3.14, see `.python-version`) automatically if it is missing.

### 2. Install dependencies

From the project root:

```bash
uv sync
```

This creates `.venv/` and installs everything from `uv.lock`, including the dev group (`ipykernel`, needed to run notebooks in VS Code).

### 3. Open the notebook in VS Code

1. Open the project folder: `code .`
2. Open `analyze-metrics-1.ipynb`.
3. Click **Select Kernel** (top right) → **Python Environments…** → choose the `.venv` interpreter (`.venv/bin/python`; on Windows `.venv\Scripts\python.exe`).
   - If `.venv` isn't listed, run **Python: Select Interpreter** from the Command Palette (`Cmd/Ctrl+Shift+P`) or reload the window.
4. Run all cells (**Run All**), or step through with `Shift+Enter`.

The notebook reads `combined_report_full_anon.csv` using a relative path, so VS Code must be opened on the project root.

### Alternative: run from the terminal

```bash
uv run jupyter lab   # requires: uv add --dev jupyterlab
```

## Managing dependencies

```bash
uv add <package>          # runtime dependency
uv add --dev <package>    # dev-only dependency
uv sync                   # re-sync the environment with the lockfile
```

## Troubleshooting

- **`ModuleNotFoundError` in the notebook:** the wrong kernel is selected. Re-select `.venv` and run `uv sync`.
- **"Install ipykernel" prompt:** run `uv sync` (the dev group includes it).
- **Python 3.14 not found:** run `uv python install 3.14`, then `uv sync`.
- **Plotly charts don't render:** make sure the Jupyter extension is up to date and `nbformat` is installed (`uv sync` covers it).
