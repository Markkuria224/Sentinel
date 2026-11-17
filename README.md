# SENTINEL — Mini SOC Analyst Dashboard

SENTINEL is a locally hosted defensive-security dashboard designed to simulate a compact Security Operations Centre (SOC) analyst workflow. It transforms authentication telemetry into prioritised alerts and provides tools for investigation, monitoring, and incident tracking.

Built with Python, Flask, SQLite, and a browser-based operational interface, SENTINEL demonstrates practical blue-team concepts through event ingestion, authentication anomaly detection, alert management, and investigation workflows.

> **Project type:** Defensive security / Mini SOC dashboard  
> **Status:** Portfolio project — V2  
> **Execution:** Local environment  
> **Purpose:** Cybersecurity learning and demonstration

---

## 🔐 Threat Posture

SENTINEL simulates a small SOC responding to risky login behaviour and suspicious authentication patterns.

It focuses on defensive monitoring, identifying suspicious activity, prioritising alerts, and supporting analyst investigation.

## 🚀 Core Capabilities

- Repeated-failure login detection and brute-force pattern analysis.
- Suspicious source-IP monitoring and authentication anomaly tracking.
- Severity-based alert prioritisation across High, Medium, and Low events.
- Live synthetic event simulation.
- Read-only collection of local Windows Security events on Windows 11.
- Windows event filtering by event ID, time range, and severity.
- Duplicate protection using computer name and Windows event record ID.
- Combined alert filtering by severity, status, source IP, and date range.
- Analyst note-taking and investigation history tied to individual alerts.
- Timeline tracking for status changes and mitigation updates.
- Hash-based navigation across Overview, Alerts, Threats, Intel, and Reports, including browser history support.
- Local-only execution for demonstrations and defensive-security learning.

## 📊 Operator Workflow

### Dashboard Overview

- Overview cards for total events, successful logins, failed logins, and suspicious IPs.
- Event metrics aggregated from stored demo, simulation, CSV, and Windows event records.
- Event trend chart covering today and the previous six UTC calendar dates.
- Severity distribution chart reflecting alerts after active alert filters are applied.

### Alert Management

- Dynamic alert filtering and live updates.
- Severity-based alert prioritisation.
- Investigation status updates.
- Analyst notes and investigation history.
- Timeline tracking for investigation progress.

### Event Ingestion

- Synthetic authentication event simulation.
- CSV log import with validation and duplicate handling.
- Native Windows Security event collection.
- Manual event collection and configurable polling.

## 🪟 Windows Security Event Collection

On Windows 11, SENTINEL can read local Security log events using the built-in Windows PowerShell `Get-WinEvent` cmdlet.

Collection is read-only and limited to the following event IDs:

| Event ID | Description |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4634 | Logoff |
| 4648 | Logon using explicit credentials |
| 4672 | Special privileges assigned to new logon |

Default collection settings:

- Lookback period: 24 hours.
- Event limit: 500 records.
- Duplicate protection using computer name and event record ID.
- Manual refresh through the **Collect now** control.
- Configurable polling interval.

Event ID, severity, and time-range filters apply to displayed records. Collection errors remain visible in the dashboard.

### Permissions and Safety

Reading the Security log may require running PowerShell or the application with administrator privileges, depending on local Windows policy. Users with appropriate Security log read permissions may not need elevation.

SENTINEL:

- Does not modify Windows Security logs.
- Does not change auditing settings or Windows security policies.
- Does not collect passwords, tokens, or user documents.
- Uses read-only Windows event collection.

On Linux and other non-Windows systems, native Windows event collection is unavailable. CSV analysis remains supported.

## 📁 CSV Log Import

Use the **Import CSV logs** panel to submit a CSV file containing these required columns:

| Column | Requirement |
|---|---|
| `timestamp` | ISO 8601 timestamp |
| `source_ip` | Valid IPv4 or IPv6 address |
| `event_type` | `login`, `auth`, or `authentication` |
| `user` | Username associated with the event |
| `status` | `success` or `failed` |
| `host` | Source host |

Import safeguards:

- Maximum file size: 2 MiB.
- Maximum records: 10,000.
- Input validation before import.
- In-memory processing.
- Duplicate detection to prevent identical rows from the same source being imported repeatedly.

## 🧱 Project Structure

| File / Directory | Purpose |
|---|---|
| `app.py` | Flask routes, API endpoints, and live simulation logic |
| `analyzer.py` | CSV validation and event normalization |
| `detector.py` | Detection rules for suspicious login and authentication patterns |
| `windows_collector.py` | Read-only Windows Security event collection and normalization |
| `database.py` | SQLite persistence, schema setup, and alert lifecycle management |
| `sample_logs.csv` | Synthetic event data for demonstrations and testing |
| `templates/` | Dashboard UI templates |
| `static/` | CSS and JavaScript frontend assets |

### Test Files

| Test File | Coverage |
|---|---|
| `test_alert_filters.py` | Alert filtering, workflow, simulation, and detection |
| `test_api_validation.py` | Invalid and malformed API requests |
| `test_csv_import.py` | CSV validation, upload limits, and duplicate imports |
| `test_dashboard_metrics.py` | Dashboard aggregates and large datasets |
| `test_database_reliability.py` | Foreign keys, alert deduplication, and database reliability |
| `test_frontend_security.py` | Malicious-content regression tests |
| `test_navigation.py` | Navigation and view mapping |
| `test_windows_collector.py` | Mocked collection, permissions, filtering, and deduplication |

## 🛠️ Technology Stack

- **Language:** Python 3
- **Backend:** Flask
- **Database:** SQLite
- **Frontend:** HTML, CSS, JavaScript
- **Charts:** Chart.js
- **Windows Integration:** PowerShell `Get-WinEvent`
- **Testing:** pytest

## ⚙️ Installation

### Prerequisites

- Python 3.11
- Git
- Windows 11 for native Windows Security event collection

### 1. Clone the repository

```bash
git clone https://github.com/Markkuria224/Sentinel.git
cd Sentinel
```

### 2. Create a virtual environment

**Windows PowerShell:**

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**Linux / macOS:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install -r requirements.txt
```

## ▶️ Run Locally

Start the application:

```bash
python app.py
```

Open your browser:

**http://127.0.0.1:5000**

The dashboard runs locally on your machine.

## 🧪 Tests

Install the test runner:

```bash
python -m pip install pytest
```

Run the test suite from the project directory:

```bash
python -m pytest -q
```

### Verified Test Results

The current verified run completed with:

- **75 passed**
- **0 failed**
- **0 skipped**
- Execution time: 50.65 seconds
- Process exit code: 0

Windows event collection tests mock PowerShell responses. Therefore, automated test results do not independently validate collection against a real Windows Security log.

A manual controlled test confirmed Windows Security event collection and duplicate handling in the tested environment. Broader Windows compatibility and reliability have not been established.

## 🕵️ Analyst Investigation Flow

1. Launch the dashboard and review the current threat picture.
2. Inspect the overview metrics and event trends.
3. Filter alerts by severity, status, date range, or suspicious IP.
4. Investigate individual alerts using their timelines and analyst notes.
5. Update case status to reflect investigation progress.
6. Review recorded investigation activity.

## 📌 Current Scope and Limitations

The Threats view currently contains the Windows Security Events panel.

Intel and Reports navigation works, but those views are currently empty states.

The current version does not implement:

- External threat-intelligence feeds.
- IOC management.
- Automated report generation.
- Network telemetry ingestion.
- External security API integrations.

SENTINEL is intentionally a local educational and portfolio project. It is not a production SOC platform or a replacement for enterprise SIEM solutions.

## 🎯 Portfolio Positioning

SENTINEL demonstrates practical defensive-security engineering through:

- Security-focused application development.
- Authentication event analysis.
- Rule-based suspicious activity detection.
- Alert prioritisation and investigation workflows.
- Database-backed event and alert management.
- Windows Security event integration.
- Input validation and automated testing.
- Full-stack development using Python and Flask.

## 🔮 Future Upgrades

Potential development areas include:

- Expanded detection rules and threat heuristics.
- Incident export and reporting views.
- Historical trend comparison and data retention.
- Role-based analyst workflows and access layers.
- Deeper investigation into related hosts, usernames, and attack paths.

---

<div align="center">

**SENTINEL — Observe. Detect. Investigate.**

Built for defensive-security learning and practical cybersecurity development.

</div>
