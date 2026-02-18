# 🏃‍♂️ Strava Custom Connector for Supermetrics

**by Léo IDIR**

Transform your Strava activities into a high-precision performance database. This project leverages the **Supermetrics Connector Builder** to create a seamless pipeline from Strava to Google Sheets and Looker Studio.

Whether you're tracking your global "World Tour" progress or deep-diving into power zones, this connector gives your fitness data the "Supermetrics treatment."

---

## 🚀 Quick Start Guide

### ⚙️ Step 1: Strava API Registration

You must register your personal instance on the Strava Developer portal to enable data access.

1. **Visit:** [Strava API Settings](https://www.strava.com/settings/api).
2. **Configure App:**
* **Application Name:** `Supermetrics Connector`
* **Category:** `Data Importer`
* **Authorization Callback Domain:** `supermetrics.com`


3. **Secure Credentials:** Note your **Client ID** and **Client Secret**. You will need these for the JSON configuration.

### 🛠️ Step 2: Deployment & Configuration

1. **Open Builder:** Navigate to the **Supermetrics Custom Connector Builder**.
2. **Inject Code:** Copy the content of the `strava.json` file from this repository.
3. **Update Keys:** Replace the following placeholders in the JSON with your actual Strava credentials:
```json
"clientId": "YOUR_STRAVA_CLIENT_ID",
"clientSecret": "YOUR_STRAVA_CLIENT_SECRET"

```


4. **How it Works:** This connector uses **Chained Requests**. It first fetches your activity list, then executes a secondary request for each Activity ID to retrieve granular telemetry like heart rate, power, and cadence.

### 📊 Step 3: Archiving to Google Sheets

Bypass API rolling windows by creating a permanent historical archive in Sheets.

1. **Launch:** Open the Supermetrics extension in Google Sheets.
2. **Query Setup:**
* **Source:** Select your custom "Strava" connector.
* **Dimensions:** `Date`, `Activity Name`, `Sport Type`, `Latitude`, `Longitude`.
* **Metrics:** `Distance`, `Moving Time`, `Avg Heart Rate`, `Avg Watts`, `Elevation Gain`.


3. **Automate:** Set a **Refresh Trigger** (e.g., daily at 05:00) to keep your database updated while you sleep.

---

## 📈 Data Transformation Tips

Once your data is in Google Sheets, use these tips for Looker Studio:

* **World Map Visualization:** Create a calculated field named `Location` with the formula:
`CONCAT(Latitude, ",", Longitude)`
*Set the data type to **Geo > Latitude, Longitude**.*
* **Unit Conversion:** Convert raw meters to kilometers:
`Distance (m) / 1000`

---

## 🔧 Troubleshooting & Maintenance

### Common Issue: Authorization Expiration

If data stops refreshing, the OAuth token may need a manual kickstart.

* **The Fix:** Ensure `tokenLifetime` in your JSON is `21600` (6 hours). If it fails, simply re-run the "Log In" flow from the Supermetrics query sidebar.

### Missing Data (Heart Rate/Watts)

If performance metrics are null:

* **Check Scopes:** You must authorize the app with `activity:read_all` during the initial login. Without this, Strava hides sensitive telemetry.
* **Device Check:** Ensure your Garmin/Wahoo/Apple Watch is actually capturing and uploading those specific metrics to Strava.

---
