# Zentra Weekly Report Dashboard

This folder is a static GitHub Pages dashboard for showing the latest Zentra 7-day historical data and 14-day forecast.

## Daily Automation

The repository includes a GitHub Actions workflow at `.github/workflows/update-dashboard.yml`.

Every day at 6:00 AM America/New_York, the workflow:

1. Installs the Python dependencies from `requirements.txt`.
2. Runs `python weekly_runner.py --dashboard-only`.
3. Updates the dashboard data in `app.js`.
4. Commits and pushes the refreshed dashboard files back to the repository.

You can also run it manually from GitHub:

1. Open the repository on GitHub.
2. Go to `Actions`.
3. Select `Update Zentra dashboard`.
4. Click `Run workflow`.

Before the workflow can run, add this repository secret:

```text
ZENTRA_API_TOKEN
GEE_FIELD_ASSET_ID
GEE_SERVICE_ACCOUNT_KEY_JSON
```

Add it in GitHub under `Settings` > `Secrets and variables` > `Actions` > `New repository secret`.

`GEE_FIELD_ASSET_ID` is the Google Earth Engine table asset for the field boundary. `GEE_SERVICE_ACCOUNT_KEY_JSON` is the full JSON key for a Google service account that has access to Earth Engine and has been granted access to the field asset.

Optional repository variables:

```text
ZENTRA_DEVICE_SN
ZENTRA_PORT_NUM
SENSOR_LATITUDE
SENSOR_LONGITUDE
GEE_PROJECT
```

If you do not set the optional variables, the script uses the defaults already in `weekly_runner_v3.py`.

## Publish on GitHub Pages

1. Commit this folder to the repository.
2. In GitHub, open `Settings` > `Pages`.
3. Set the source to the repository branch and `/ (root)` folder.
4. Save. GitHub will provide the public dashboard URL.

## Connect the Signup Form

Static GitHub Pages cannot store email addresses by itself. By default, the form opens a prepared email to `dialameh.babak@gmail.com`.

To submit signups to a real endpoint instead, edit `app.js`:

```js
const SIGNUP_ENDPOINT = "https://your-form-or-api-endpoint.example";
```

The page will send:

```json
{
  "email": "name@example.com",
  "source": "zentra-dashboard"
}
```

Good endpoint options include Formspree, Google Apps Script, Airtable Forms, or a small GitHub Actions-backed API.
