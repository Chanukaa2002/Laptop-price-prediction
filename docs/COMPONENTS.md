# Components and Structure

## Python Modules

### `web_app/app.py`
- **`prediction(lst: list) -> numpy.ndarray`**: Loads `model/predictor.pickle` and returns a model prediction for a single feature vector.
- **`index()` route (`GET`, `POST`)**: Root route that renders the form and, on `POST`, computes and displays the predicted price in LKR.

#### Programmatic example
```python
from web_app.app import prediction

# Example feature vector encoding:
# [ram(int), weight(float), touchscreen(0/1), ips(0/1),
#  one-hot company (9), one-hot typename (6), one-hot opsys (4),
#  one-hot cpuname (5), one-hot gpuname (3)]
# This vector must match the order in `index()`.

features = [16, 1.35, 1, 1] + \
    [0,0,0,0,1,0,0,0,0] + \
    [0,0,0,0,1,0] + \
    [1,0,0,0] + \
    [0,0,1,0,0] + \
    [0,1,0]

pred = prediction(features)
# The web route scales this value by ~354 before showing it
print(float(pred[0]))
```

## Templates

### `web_app/templates/index.html`
- Displays a form with the following inputs:
  - `ram` (text)
  - `weight` (text)
  - `company` (select): `acer, apple, asus, dell, hp, lenovo, msi, toshiba, other`
  - `typename` (select): `2in1convertible, gaming, netbook, notebook, ultrabook, workstation`
  - `opsys` (select): `windows, mac, linux, other`
  - `cpuname` (select): `intelcorei3, intelcorei5, intelcorei7, amd, other`
  - `gpuname` (select): `intel, amd, nvidia`
  - `touchscreen` (checkbox)
  - `ips` (checkbox)
- After submission, shows `Estimated Price : LKR {{ pred_value }}` when available.

## Static Assets

### `web_app/static/style.css`
- Styles the prediction form and layout. Linked in the template via `/static/style.css`.

## Model Artifact

### `web_app/model/predictor.pickle`
- Serialized scikit-learn model used by `prediction()`.
- Must be present and compatible with the feature vector built in `index()`.