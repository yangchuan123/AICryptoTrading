# AGENTS.md

## Cursor Cloud specific instructions

### Project Overview

AICryptoTrading is a Jupyter notebook-based ML project that uses LSTM neural networks to predict cryptocurrency prices (BTC/ETH). Originally written for Python 2.7 with old Keras. See `README.md` for more details.

### Development Environment

- **Python**: 3.11 (installed via deadsnakes PPA), used through a virtualenv at `/workspace/.venv`
- **Key packages**: TensorFlow 2.15.1, Keras 2.15.0, pandas, numpy, scikit-learn, matplotlib, Jupyter
- **Activate venv**: `source /workspace/.venv/bin/activate`

### Compatibility Notes

1. **CuDNNLSTM/CuDNNGRU**: These layers were removed from modern Keras. A `.pth` compatibility shim in the venv site-packages aliases them to standard LSTM/GRU. The notebook imports them but only uses regular LSTM in the actual model.

2. **`units/2` float division**: In Python 3, `units/2` returns `25.0` (float) instead of `25` (int). Keras 2.15 LSTM may raise `TypeError`. Use `int(units/2)` or `units//2` when running interactively.

3. **Last 3 notebook cells** (cells 22-24) use Python 2-only constructs (`urllib2`, `print` without parentheses) and will not execute on Python 3. These are live Poloniex API prediction cells and are not part of the core ML pipeline.

### Running the Notebook

```bash
source /workspace/.venv/bin/activate
jupyter notebook --no-browser --ip=0.0.0.0 --port=8888 --NotebookApp.token= --notebook-dir=/workspace
```

Select the **"Python 3.11 (AICryptoTrading)"** kernel in Jupyter.

### Running the ML Pipeline via CLI

```bash
source /workspace/.venv/bin/activate
cd /workspace
python -c "from config import CONFIG; from utils import series_to_supervised; print(CONFIG)"
```

### No Automated Tests

This project has no test framework or test files. Validation is done by running the Jupyter notebook cells and observing training loss convergence and prediction outputs.

### No Linter Configuration

No linter (pylint, flake8, etc.) is configured for this project.
