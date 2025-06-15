# Lab 2 – Web App Threat Detection with Azure Monitor and KQL

**Student Name**: Maryam Khalaf  
**Student Number**: 041188885

---

## 📘 What I Learned

During this lab, I learned how to deploy a Python Flask web application to Azure App Service using Local Git deployment. I configured Azure Monitor and Log Analytics to collect diagnostic logs from the application and wrote Kusto Query Language (KQL) queries to analyze those logs.

I practiced detecting suspicious login behavior (e.g., failed logins), created a KQL-based alert rule, and integrated it with an Azure Action Group to trigger an email notification when threats are detected.

---

## ⚠️ Challenges Faced

- Initially encountered quota errors when trying to deploy to the Free (F1) App Service tier and had to change regions.
- Diagnostic logging wasn't active at first, requiring manual configuration to send logs to Log Analytics.
- REST Client tests initially failed because the deployed app had a randomized name suffix, causing URL mismatches.
- Understanding the alert logic and tying it correctly to the KQL query and action group took experimentation.

---

## 🚀 Real-World Improvements

In a production environment, I would improve the detection logic by:
- Tracking repeated failed logins from the same IP over time
- Adding geo-location-based anomaly detection
- Including device fingerprinting to detect unusual access patterns
- Integrating with Microsoft Sentinel for advanced threat correlation

---

## 🧠 KQL Query Used

```kql
AppServiceConsoleLogs
| where TimeGenerated > ago(5m)
| where Message has "Failed login attempt"
| summarize FailedAttempts=count() by bin(TimeGenerated, 5m)
```

### Explanation:

- **`AppServiceConsoleLogs`**: This table stores the console log entries from the Azure App Service.
- **`where TimeGenerated > ago(5m)`**: Filters the logs to only include entries from the last 5 minutes.
- **`where Message has "Failed login attempt"`**: Filters only the logs that contain failed login attempts.
- **`summarize FailedAttempts=count() by bin(TimeGenerated, 5m)`**: Groups the filtered logs into 5-minute buckets and counts how many failed attempts occurred in each bucket.

This query helps detect brute-force login activity by checking how often failed login attempts happen within short time frames. It’s useful for triggering alerts when activity becomes suspicious.

---

## 🎥 YouTube Demo

**

---

## 📂 Included Files

- `app.py` — Main Flask application with `/login` route
- `requirements.txt` — Python dependencies
- `test-app.http` — REST Client test file for triggering login requests
- `README.md` — Lab report (this file)

---

