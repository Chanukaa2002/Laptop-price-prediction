# Laptop-price-prediction

A simple Flask web app that predicts laptop prices from user-provided specifications using a pre-trained scikit-learn model.

## Quickstart

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
export FLASK_APP=web_app/app.py
flask run --debug
```

Open `http://127.0.0.1:5000` and submit the form to get an estimated price in LKR.

## Documentation
- API: `docs/API.md`
- Usage: `docs/USAGE.md`
- Components: `docs/COMPONENTS.md`
- OpenAPI spec: `docs/OPENAPI.yaml`

## Deployment
Uses Gunicorn with the entrypoint defined in `web_app/Procfile`.

```bash
cd web_app
gunicorn app:app
```
