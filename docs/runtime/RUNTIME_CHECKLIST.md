# Runtime Checklist

This checklist is for future runtime work. It summarizes the KRONOS-002 proven path and stop conditions without authorizing new runtime audit scope.

## Before Runtime Work

- Read `docs/project_status.md`, `docs/runtime/KRONOS_RUNTIME_AUDIT.md`, and `docs/runtime/ENVIRONMENT.md`.
- Confirm the working tree is clean before runtime work.
- Confirm the package authorizes runtime execution before installing dependencies, running tests, downloading Hugging Face files, or running inference.
- Do not run broker/exchange/API/paper/live paths.

## Environment Setup

Use Python 3.11 when reproducing the currently proven path. Python 3.10 is allowed by project direction but remains unproven by KRONOS-002.

Windows PowerShell:

```powershell
py -3.11 -m venv ..\.venv-kronos-audit
..\.venv-kronos-audit\Scripts\Activate.ps1
python -m pip install --upgrade pip
```

Linux/macOS:

```bash
python3.11 -m venv ../.venv-kronos-audit
source ../.venv-kronos-audit/bin/activate
python -m pip install --upgrade pip
```

## Install

Windows PowerShell:

```powershell
python -m pip install -r requirements.txt
python -m pip install pytest
```

Linux/macOS:

```bash
python -m pip install -r requirements.txt
python -m pip install pytest
```

Do not modify `requirements.txt`, tests, examples, or model code to make runtime pass unless a later package explicitly authorizes that change.

## Test Commands

Current proven targeted regression:

Windows PowerShell:

```powershell
python -m pytest -q tests/test_kronos_regression.py
```

Linux/macOS:

```bash
python -m pytest -q tests/test_kronos_regression.py
```

Full pytest currently fails on `qlib` collection unless `qlib` policy is resolved:

```bash
python -m pytest -q
```

Do not patch tests or install `qlib` only to make full collection pass unless a future package explicitly approves that work.

## Hugging Face Cache

- Pinned tokenizer: `NeoQuasar/Kronos-Tokenizer-base` at revision `0e0117387f39004a9016484a186a908917e22426`.
- Pinned model: `NeoQuasar/Kronos-small` at revision `901c26c1332695a2a8f243eb2f37243a37bea320`.
- Expected cache location is the user Hugging Face cache, typically `%USERPROFILE%\.cache\huggingface\hub` on Windows.
- Windows symlink degradation warnings may occur and did not block KRONOS-002 targeted regression.

Do not commit cache files, checkpoints, generated model files, or credentials.

## Repeatability Settings

KRONOS-002 fixed-seed repeatability used:

- `device=cpu`
- `sample_count=1`
- `top_k=1`
- `top_p=1.0`
- `T=1.0`
- `seed=123`
- `context_len=256`
- `pred_len=8`
- Test fixture: `tests/data/regression_input.csv`
- Model/tokenizer revisions from `tests/test_kronos_regression.py`

## Cleanup

Before final validation or commit, remove generated runtime artifacts from the repo tree:

- virtual environments
- `.pytest_cache`
- `__pycache__`
- generated `.png` / `.pdf` outputs
- Hugging Face caches or checkpoint files
- scratch scripts
- large model files such as `model.safetensors`, `.safetensors`, or `pytorch_model*`

Do not commit venv/cache/checkpoints/`__pycache__`/`.pytest_cache`/generated outputs.

## Report-Back Items

Report these facts after authorized runtime work:

- Branch/base.
- Python version and executable used.
- Virtual environment location.
- Dependency install result.
- Whether `pytest` was installed outside `requirements.txt`.
- Full pytest result, if run.
- Targeted regression result, if run.
- Hugging Face access/cache behavior.
- Repeatability settings and result, if run.
- Generated artifacts created and cleaned.
- Boundary check for source, tests, examples, dependencies, data, credentials, strategy, risk, portfolio, and execution paths.

## Stop Conditions

Stop and report before continuing if:

- Python 3.11 is unavailable and Python 3.10 is unavailable.
- Dependency install fails.
- Hugging Face requires credentials or gated-term acceptance.
- Runtime would require modifying `requirements.txt`, tests, examples, or model code.
- Generated files cannot be cleaned before commit.
- Any action would touch strategy/risk/portfolio/execution/API/credentials.
