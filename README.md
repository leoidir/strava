# Strava Custom Connector for Supermetrics

**Developer:** Léo IDIR

This repository provides a high-fidelity JSON configuration for building a custom Strava API connector via Supermetrics. The integration is engineered to bypass API extraction limits by establishing a persistent data pipeline into Google Sheets, enabling long-term performance tracking and custom analytics.

## ⚙️ Step 1: Strava API Registration

To authenticate the connector, you must register your application instance on the Strava Developer portal.

1. **Register:** Visit [Strava API Settings](https://www.strava.com/settings/api).
2. **App Configuration:**
* **Application Name:** `Supermetrics Connector`
* **Category:** `Data Importer`
* **Authorization Callback Domain:** `supermetrics.com`


3. **Credentials:** Secure your **Client ID** and **Client Secret**. These act as the "keys" for your JSON configuration.

---

## 🛠️ Step 2: Deployment & Configuration

1. **Initialize:** Open the **Supermetrics Custom Connector Builder**.
2. **Code Injection:** Copy the content of the `strava.json` file from this repository.
3. **Credential Update:** Replace the following placeholders in the JSON with your Strava credentials:
```json
"clientId": "YOUR_STRAVA_CLIENT_ID",
"clientSecret": "YOUR_STRAVA_CLIENT_SECRET"

```


4. **Chained Requests:** This connector is optimized to first fetch the activity list and then execute a secondary "chained" request for each activity ID to retrieve granular telemetry (heart rate, power, cadence).

---

## 📊 Step 3: Automated Archiving (Google Sheets)

By routing data into Google Sheets, you create a permanent historical archive that persists even if Strava's API availability changes.

1. **Launch:** Open the Supermetrics extension in Google Sheets.
2. **Query Setup:**
* **Source:** Select your custom "Strava" connector.
* **Dimensions:** Date, Activity Name, Sport Type, Latitude, Longitude.
* **Metrics:** Distance, Moving Time, Avg Heart Rate, Avg Watts, Elevation Gain.


3. **Scheduling:** Set a **Refresh Trigger** (e.g., daily at 05:00) to automate the data ingestion.

---

## 🔧 Step 4: Troubleshooting & Maintenance

### Common Issue: Authorization Expiration

If your data stops refreshing, it is likely due to an expired OAuth token.

* **The Fix:** Ensure the `tokenLifetime` in your JSON is set to `21600` (6 hours). The connector is designed to use `refresh_token` automatically, but if a manual re-auth is needed, simply re-run the "log in" flow from the Supermetrics query side-bar.

### Missing Data Fields

If metrics like **Heart Rate** or **Watts** are showing as null:

* **Check Scopes:** Verify that you authorized the app with `activity:read_all`. Without this, Strava filters out sensitive performance telemetry.
* **Device Compatibility:** Ensure the recording device (Garmin, Wahoo, etc.) actually writes these fields to the Strava activity.

---

## 📈 Data Transformation Tips

To use this data in Looker Studio or PowerBI:

* **Location:** Create a calculated field using `CONCAT(Latitude, ",", Longitude)` to generate a valid Geo-point.
* **Distance:** Convert the raw meters (`m_dist`) to kilometers by creating a formula: `Distance (m) / 1000`.

---
