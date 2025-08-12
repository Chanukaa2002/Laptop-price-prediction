# Usage Guide

## Prerequisites
- Python 3.10+
- pip

## Setup
```bash
# From repo root
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Run Locally
```bash
export FLASK_APP=web_app/app.py
export FLASK_RUN_PORT=5000
flask run --debug
# Open http://127.0.0.1:5000
```

Alternatively, run with Gunicorn (mirrors `web_app/Procfile`):
```bash
cd web_app
gunicorn app:app
```

## Using the Web UI
- Go to `/` and fill the form fields:
  - RAM (GB), Weight (Kg), Company, Type Name, Operating System, CPU, GPU, Touch Screen, IPS.
- Submit to see the estimated price in LKR.

## Using cURL
See `docs/API.md` for the full example. Minimal example:
```bash
curl -X POST http://127.0.0.1:5000/ -H 'Content-Type: application/x-www-form-urlencoded' \
  --data 'ram=8&weight=1.8&company=hp&typename=notebook&opsys=windows&cpuname=intelcorei5&gpuname=intel'
```

## Environment and Files
- Model file: `web_app/model/predictor.pickle` must exist.
- Static assets: `web_app/static/style.css`.
- Templates: `web_app/templates/index.html`.

## Troubleshooting
- If you see model loading errors, verify `web_app/model/predictor.pickle` is present and generated with the same scikit-learn version as in `requirements.txt`.
- If static styles do not load, ensure Flask is serving from `/static` and files exist under `web_app/static/`.