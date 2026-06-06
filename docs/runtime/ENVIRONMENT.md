# Runtime Environment Guidance

This guidance records the known PARTIAL runtime path from KRONOS-002. It is a durable summary for future setup and runtime work; it does not rerun or expand the audit.

## Current Runtime Status

Runtime status: PARTIAL, based on KRONOS-002.

Python 3.11 CPU runtime worked for dependency install, targeted regression, Hugging Face access, minimal inference, and exact fixed-seed repeatability. Full pytest collection, README example data, and GPU/MPS behavior remain unresolved.

## Supported / Proven Path

This is the only currently documented proven path. It is not a claim that other environments are invalid.

- OS/platform audited: Windows 10.
- Python: 3.11.9.
- Virtual environment: outside repo at `C:\GitHub\.venv-kronos-audit`.
- Dependency install: `python -m pip install -r requirements.txt`.
- Additional local audit install: `python -m pip install pytest`.
- Targeted regression: `python -m pytest -q tests/test_kronos_regression.py`.

## Python Version Policy

- Python 3.11 is currently proven by KRONOS-002.
- Python 3.10 is allowed by project direction and README guidance, but was unavailable in KRONOS-002 and remains unproven in this repo audit.
- Python 3.13/3.14 must not be used as runtime-proof versions until separately audited.

## Virtual Environment Location

Create audit/dev virtual environments outside the repo working tree.

Windows PowerShell:

```powershell
py -3.11 -m venv ..\.venv-kronos-audit
..\.venv-kronos-audit\Scripts\Activate.ps1
python --version
python -m pip install --upgrade pip
```

Linux/macOS:

```bash
python3.11 -m venv ../.venv-kronos-audit
source ../.venv-kronos-audit/bin/activate
python --version
python -m pip install --upgrade pip
```

Do not commit virtual environments. If a virtual environment must be created inside the repo, remove it before final validation and explain why outside-repo was not possible.

## Dependency Installation

Install repository dependencies with:

```bash
python -m pip install -r requirements.txt
```

Do not modify `requirements.txt` in KRONOS-003. Do not add lock files in KRONOS-003. Dependency pinning and lock strategy are future package decisions.

## Pytest Requirement

`pytest` is required to run tests but was not installed by `requirements.txt` during KRONOS-002.

In KRONOS-002, `pytest` was installed locally in the audit environment with:

```bash
python -m pip install pytest
```

Do not add `pytest` to `requirements.txt` in KRONOS-003. Whether `pytest` belongs in requirements, dev requirements, `pyproject` extras, or docs-only guidance remains a future decision.

## Test Command Guidance

Current full-suite command:

```bash
python -m pytest -q
```

Current known result: full pytest collection fails because `finetune/qlib_test.py` imports `qlib` and `qlib` is not installed by `requirements.txt`.

Current proven targeted regression path:

```bash
python -m pytest -q tests/test_kronos_regression.py
```

The targeted regression command is the current proven runtime-regression command from KRONOS-002. Do not patch tests or install `qlib` in KRONOS-003. The `qlib` test collection question is deferred.

## Hugging Face Access and Cache

KRONOS-002 accessed these pinned public/ungated repositories:

- `NeoQuasar/Kronos-Tokenizer-base`, revision `0e0117387f39004a9016484a186a908917e22426`.
- `NeoQuasar/Kronos-small`, revision `901c26c1332695a2a8f243eb2f37243a37bea320`.

Downloads were cached outside the repo under the user Hugging Face cache, typically `%USERPROFILE%\.cache\huggingface\hub` on Windows. Windows emitted a symlink degradation warning for the Hugging Face cache, but the warning did not block targeted regression.

Do not commit Hugging Face cache files or checkpoints. Do not add credentials. Do not accept gated terms in KRONOS-003.

## Model-Card / License Observation

KRONOS-002 observed public metadata with MIT license fields/tags for the pinned tokenizer and model repositories.

This is not production-use legal approval. Future production or commercial use still requires separate legal and terms review.

## Known Limitations

- Full pytest fails without `qlib`.
- The README example fails from repo root due import-path behavior.
- The README example fails from `examples/` because `./data/XSHG_5min_600977.csv` is missing.
- `prediction_wo_vol` example was skipped because it hardcodes CUDA and the audit machine was CPU-only.
- GPU/MPS behavior is untested.
- CPU memory was not separately measured.
- Latency evidence is narrow and cached CPU-only.

## Forbidden Changes

- No requirements changes.
- No test patches.
- No example patches.
- No model/tokenizer changes.
- No data adapters.
- No forecast artifacts.
- No strategy/risk/portfolio/execution work.
- No credentials.

## What This File Does Not Prove

- It does not prove full test-suite health.
- It does not prove GPU/MPS behavior.
- It does not prove README example usability.
- It does not prove production license approval.
- It does not prove alpha value.
- It does not make Kronos output order, risk, or portfolio authority.

Kronos output remains forecast input only.

## Next Runtime Follow-Ups

Future package options:

- Resolve `qlib` test collection policy.
- Resolve example data / example working-directory behavior.
- Run CUDA/GPU audit on suitable hardware.
- Add memory measurement in a future audit if needed.
- Create environment lock/dev dependency strategy in a later approved package.
