# Laptop Price Predictor API

## Base URL

- Local development: `http://127.0.0.1:5000/`
- Production (Gunicorn): as configured by your platform, using the `web_app/Procfile` entrypoint `app:app`.

## Endpoints

### GET /
- **Description**: Render the prediction form UI.
- **Response**: HTML page (`templates/index.html`).

### POST /
- **Description**: Accepts laptop specs and returns predicted price rendered in HTML.
- **Content-Type**: `application/x-www-form-urlencoded`
- **Request body fields**:
  - `ram` (string, required): RAM in GB. Example: `16`.
  - `weight` (string, required): Weight in kilograms. Example: `1.5`.
  - `company` (enum, required): One of `acer | apple | asus | dell | hp | lenovo | msi | toshiba | other`.
  - `typename` (enum, required): One of `2in1convertible | gaming | netbook | notebook | ultrabook | workstation`.
  - `opsys` (enum, required): One of `windows | mac | linux | other`.
  - `cpuname` (enum, required): One of `intelcorei3 | intelcorei5 | intelcorei7 | amd | other`.
  - `gpuname` (enum, required): One of `intel | amd | nvidia`.
  - `touchscreen` (checkbox, optional): Present if selected.
  - `ips` (checkbox, optional): Present if selected.
- **Response**: HTML page with estimated price injected into `{{ pred_value }}`. Price shown in LKR.

## Request/Response Examples

### cURL (form submission)
```bash
curl -X POST http://127.0.0.1:5000/ \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'ram=16' \
  --data-urlencode 'weight=1.35' \
  --data-urlencode 'company=lenovo' \
  --data-urlencode 'typename=ultrabook' \
  --data-urlencode 'opsys=windows' \
  --data-urlencode 'cpuname=intelcorei7' \
  --data-urlencode 'gpuname=intel' \
  --data-urlencode 'touchscreen=on' \
  --data-urlencode 'ips=on'
```

### Python (requests)
```python
import requests

resp = requests.post(
    'http://127.0.0.1:5000/',
    data={
        'ram': '16',
        'weight': '1.35',
        'company': 'lenovo',
        'typename': 'ultrabook',
        'opsys': 'windows',
        'cpuname': 'intelcorei7',
        'gpuname': 'intel',
        'touchscreen': 'on',
        'ips': 'on',
    },
)
print(resp.status_code)
print(resp.text[:500])  # HTML content with rendered price
```

### HTML form usage
The default UI at `/` provides a complete form. Submit it to see the predicted price displayed under "Estimated Price".

## Notes
- The app computes a feature vector from form fields, makes a prediction using `model/predictor.pickle`, then scales the result: `round(pred[0], 2) * 354`.
- If the model file is missing or incompatible, the server will raise an error on POST.