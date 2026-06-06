# Kronos Runtime Audit

## A. Scope

KRONOS-002 audited whether the current Kronos fork can be installed, tested, loaded, and exercised reproducibly under Python 3.10 or 3.11 without modifying repo code, tests, examples, dependencies, data, model/tokenizer code, or protected trading/runtime paths.

This audit used the current `toobigtofailbot-maker/Kronos` fork state and treated Kronos output only as forecast input. No strategy, risk, portfolio, OMS, execution, broker, exchange, credential, data adapter, fine-tuning, model refactor, or tokenizer refactor work was performed.

## B. Environment

- OS/platform: Windows-10-10.0.26200-SP0.
- Repo path: `C:\GitHub\Kronos`.
- Audit venv: `C:\GitHub\.venv-kronos-audit` outside the repo working tree.
- Selected Python: Python 3.11.9.
- Selected Python executable: `C:\GitHub\.venv-kronos-audit\Scripts\python.exe`.
- Pip after upgrade: `pip 26.1.2`.
- Default `python` on PATH: Python 3.14.3, not used for runtime proof.
- Python 3.10: unavailable via `py -3.10`.

Key package versions after dependency install and local pytest install:

```text
torch: 2.12.0
numpy: 2.4.6
pandas: 2.2.2
huggingface_hub: 0.33.1
safetensors: 0.6.2
pytest: 9.0.3
einops: 0.8.1
tqdm: 4.67.1
matplotlib: 3.9.3
```

## C. Repo State Inspected

- Branch used for audit work: `runtime/kronos-002-runtime-audit`.
- Base: `origin/master` at `9b14ce8`, merge commit for KRONOS-001.
- Required KRONOS-001 docs were present under `docs/`.
- No open PRs were reported by `gh pr list --repo toobigtofailbot-maker/Kronos --state open --json number,title,headRefName,url`.
- Working tree was clean before branch creation and before audit edits.

Repo-truth findings before runtime:

- README recommends Python 3.10+.
- `requirements.txt` declares `numpy`, `pandas`, `torch>=2.0.0`, `einops==0.8.1`, `huggingface_hub==0.33.1`, `matplotlib==3.9.3`, `pandas==2.2.2`, `tqdm==4.67.1`, and `safetensors==0.6.2`.
- Regression tests exist in `tests/test_kronos_regression.py`.
- Regression fixtures exist under `tests/data/`: `regression_input.csv`, `regression_output_256.csv`, and `regression_output_512.csv`.
- Tests use `NeoQuasar/Kronos-Tokenizer-base` at revision `0e0117387f39004a9016484a186a908917e22426`.
- Tests use `NeoQuasar/Kronos-small` at revision `901c26c1332695a2a8f243eb2f37243a37bea320`.
- README/example scripts expect `./data/XSHG_5min_600977.csv`, but `data/` is missing in this checkout.
- `docs/runtime/` did not exist before KRONOS-002.

## D. Python Interpreter Selection

Interpreter discovery:

```text
python --version -> Python 3.14.3
py -0p -> Python 3.14 and Python 3.11 installed
py -3.11 --version -> Python 3.11.9
py -3.10 --version -> no matching runtime installed
where.exe python -> C:\Users\dersu\AppData\Local\Microsoft\WindowsApps\python.exe
```

Decision: Python 3.11.9 was selected because it is an approved audit version and available locally. Python 3.14.3 was not used for runtime proof.

## E. Dependency Install Result

Command:

```text
C:\GitHub\.venv-kronos-audit\Scripts\python.exe -m pip install -r requirements.txt
```

Result: success.

Important observations:

- `pip` downloaded and installed Windows CPU torch `2.12.0`.
- The duplicate `pandas` declaration resolved to `pandas 2.2.2`.
- `pytest` was not installed by `requirements.txt`.

Because tests required pytest, the following local audit-environment-only command was run:

```text
C:\GitHub\.venv-kronos-audit\Scripts\python.exe -m pip install pytest
```

Result: success, installing `pytest 9.0.3`. `requirements.txt` was not modified.

## F. Pytest / Regression Test Result

Full pytest command:

```text
C:\GitHub\.venv-kronos-audit\Scripts\python.exe -m pytest -q
```

Result: failed during collection in 10.37 seconds.

Failure excerpt:

```text
ERROR collecting finetune/qlib_test.py
ModuleNotFoundError: No module named 'qlib'
```

Approved targeted regression command:

```text
C:\GitHub\.venv-kronos-audit\Scripts\python.exe -m pytest -q tests\test_kronos_regression.py
```

Result: passed.

```text
4 passed, 2 warnings in 56.96s
ELAPSED_SECONDS=58.60
```

Hugging Face downloads/cache access occurred during the regression test. Windows emitted symlink degradation warnings for the user-level Hugging Face cache, but the regression test still passed.

## G. Hugging Face Access and Cache Behavior

Accessed checkpoints:

- `NeoQuasar/Kronos-Tokenizer-base`, revision `0e0117387f39004a9016484a186a908917e22426`.
- `NeoQuasar/Kronos-small`, revision `901c26c1332695a2a8f243eb2f37243a37bea320`.

Access result:

- Authentication required: no.
- Gated-term acceptance required: no.
- Download/cache succeeded.
- Cache location observed: `C:\Users\dersu\.cache\huggingface\hub`.
- Tokenizer cache size observed: about 15.11 MB.
- Model cache size observed: about 94.40 MB.

Cache warning:

```text
huggingface_hub cache-system uses symlinks by default ... your machine does not support them ... C:\Users\dersu\.cache\huggingface\hub\models--NeoQuasar--Kronos-Tokenizer-base
```

The same symlink warning was observed for `Kronos-small`. No Hugging Face cache files were created or retained inside the repo tree.

## H. Hugging Face Model-Card / License Observation

Public metadata was visible through `huggingface_hub.model_info` for both pinned revisions.

`NeoQuasar/Kronos-Tokenizer-base`:

- SHA: `0e0117387f39004a9016484a186a908917e22426`.
- Private: false.
- Gated: false.
- License field: `mit`.
- Tags include `license:mit`, `Finance`, `Candlestick`, `K-line`, and `time-series-forecasting`.
- Public files listed included `README.md`, `config.json`, and `model.safetensors`.

`NeoQuasar/Kronos-small`:

- SHA: `901c26c1332695a2a8f243eb2f37243a37bea320`.
- Private: false.
- Gated: false.
- License field: `mit`.
- Tags include `license:mit`, `Finance`, `Candlestick`, `K-line`, and `time-series-forecasting`.
- Public files listed included `README.md`, `config.json`, and `model.safetensors`.

Observation only: runtime access and public MIT metadata are not production-use legal approval.

## I. Example Inference Result

Requested command from repo root:

```text
C:\GitHub\.venv-kronos-audit\Scripts\python.exe examples\prediction_example.py
```

Result: failed before model/data work because the script appends `../` relative to the current working directory.

Failure excerpt:

```text
ModuleNotFoundError: No module named 'model'
```

Second safe attempt from `examples/` with `MPLBACKEND=Agg` and the absolute audit Python:

```text
C:\GitHub\.venv-kronos-audit\Scripts\python.exe prediction_example.py
```

Result: failed after model/tokenizer load because required example data is missing from the repo checkout.

Failure excerpt:

```text
FileNotFoundError: [Errno 2] No such file or directory: './data/XSHG_5min_600977.csv'
```

`examples/prediction_wo_vol_example.py` was inspected but not run because it hardcodes `device="cuda:0"` and the audit environment has no CUDA. It also uses the same missing example data path.

Minimal inference did run through the regression test path and the fixed-seed repeatability check in Section J.

## J. Fixed-Seed Repeatability Result

Repeatability command: one-off Python sent through stdin; no scratch script was committed.

Data and configuration:

- Data: `tests/data/regression_input.csv`.
- Tokenizer: `NeoQuasar/Kronos-Tokenizer-base`, revision `0e0117387f39004a9016484a186a908917e22426`.
- Model: `NeoQuasar/Kronos-small`, revision `901c26c1332695a2a8f243eb2f37243a37bea320`.
- Seed: `123` for Python `random`, NumPy, and Torch before each prediction.
- Device: CPU.
- Context length: 256.
- Prediction length: 8.
- `sample_count=1`, `top_k=1`, `top_p=1.0`, `T=1.0`.
- Comparison: exact NumPy array equality and max absolute difference.

Result:

```text
load_seconds=0.5771
run1_seconds=0.7667
run2_seconds=0.8015
exact_equal=True
max_abs_diff=0
shape=(8, 6)
device=cpu context_len=256 pred_len=8 sample_count=1 top_k=1 top_p=1.0 seed=123
```

Fixed-seed CPU repeatability was demonstrated exactly for the audited minimal path.

## K. CPU / GPU / MPS Findings

Torch capability probe:

```text
platform: Windows-10-10.0.26200-SP0
torch: 2.12.0+cpu
cuda_available: False
cuda_device_count: 0
mps_available: False
```

CPU behavior:

- Dependency install succeeded.
- Pinned regression tests passed on CPU.
- Fixed-seed repeatability passed exactly on CPU.

GPU/MPS behavior:

- CUDA unavailable.
- MPS unavailable.
- No GPU or MPS inference behavior was tested.

## L. Latency and Memory Notes

Measured narrow CPU path:

- Model/tokenizer: pinned `Kronos-small` plus `Kronos-Tokenizer-base`.
- Device: CPU.
- Data: `tests/data/regression_input.csv`.
- Context length: 256.
- Forecast horizon: 8.
- `sample_count=1`, `top_k=1`, `top_p=1.0`, `T=1.0`.
- Cached load time in repeatability run: about 0.58 seconds.
- Prediction run times: about 0.77 seconds and 0.80 seconds.

Regression test suite:

- 4 targeted regression tests completed in 56.96 seconds.

Memory:

- CPU memory was not separately measured.
- GPU memory was not applicable because CUDA was unavailable.
- No additional memory-profiling dependencies were installed.

## M. Failure Modes

- Full `pytest -q` fails during collection because `finetune/qlib_test.py` imports `qlib`, which is not installed by `requirements.txt`.
- `examples/prediction_example.py` fails from repo root because `model` is not importable with the script's current relative `sys.path` behavior.
- `examples/prediction_example.py` fails from `examples/` because `./data/XSHG_5min_600977.csv` is missing from this checkout.
- `examples/prediction_wo_vol_example.py` was skipped because it hardcodes CUDA in a CPU-only environment and uses the same missing data path.
- GPU/MPS runtime behavior remains untested because the audit environment exposes neither CUDA nor MPS.
- Hugging Face cache uses a degraded no-symlink mode on Windows, but access and regression execution still succeeded.

## N. Repo Modification Check

Runtime commands created local `.pytest_cache` and `__pycache__` directories. These were removed before final validation.

The audit venv was created outside the repo at `C:\GitHub\.venv-kronos-audit`.

Downloaded Hugging Face cache files were stored under `C:\Users\dersu\.cache\huggingface\hub`, not under the repo.

No source, model, tokenizer, dependency, test, example, data, broker/exchange, credential, strategy, risk, portfolio, OMS, execution, or forecast artifact files were modified.

## O. Runtime Viability Classification

Runtime viability classification: `PARTIAL`.

Decision basis:

- Python 3.11 environment creation succeeded.
- Repo dependencies installed without modifying `requirements.txt`.
- Targeted pinned regression tests passed.
- Hugging Face model/tokenizer access worked without credentials or gated acceptance.
- Minimal CPU inference ran.
- Fixed-seed CPU repeatability was demonstrated exactly.
- No repo code, tests, examples, dependencies, or protected runtime paths were modified.

Conservative limitation basis:

- Full `pytest -q` does not pass because of missing `qlib` for `finetune/qlib_test.py`.
- README example inference is blocked by import working-directory behavior and missing example data.
- GPU/MPS behavior is unavailable and untested.
- Latency/memory evidence covers only a narrow cached CPU path.

## P. Next Recommended Package

KRONOS-003: environment/runtime guidance update based on audit.

Suggested scope for KRONOS-003:

- Document the supported Python 3.11 setup path.
- Document that pytest is needed for tests but absent from `requirements.txt`.
- Clarify full-suite versus targeted-regression test expectations.
- Document example data requirements and working-directory behavior.
- Preserve the no-strategy/no-risk/no-execution boundaries.

## Q. Open Questions

- Should `qlib` test collection be isolated, documented as optional, or supported through a separate development dependency path?
- Should example data be restored, documented as external, or examples updated in a later approved package?
- Should GPU/CUDA runtime be audited on a machine with CUDA available?
- Should memory measurement be added in a later audit without introducing extra dependencies?
