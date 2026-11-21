# 🏋️ GymFlow — Gym Membership Automation System

**GymFlow** is a zero-budget, end-to-end gym membership management and administrative automation system built with **self-hosted n8n**, **Google Sheets**, and **Telegram** (architected with a decoupled notification layer ready for **WhatsApp Business Cloud API**).

Designed to replace manual paper registers and spreadsheet tracking for small and medium-sized gyms, GymFlow automates member onboarding, expiry date calculation, multi-stage renewal alerts, payment reconciliation, and daily owner revenue summaries.

---

## 🏗️ System Architecture

```text
┌───────────────────────────┐      ┌───────────────────────────┐
│  Member Registration Form │      │ Payment Registration Form │
│         (WF-01)           │      │         (WF-03)           │
└─────────────┬─────────────┘      └─────────────┬─────────────┘
              │                                  │
              ▼                                  ▼
┌──────────────────────────────────────────────────────────────┐
│               n8n Automation Engine (Docker)                 │
│  • ID Generation & Expiry Math    • Member Lookup & Status   │
│  • Daily Expiry Scanner (9 AM)    • Revenue Aggregation      │
└─────────────┬──────────────────────────────────┬─────────────┘
              │                                  │
              ▼                                  ▼
┌───────────────────────────┐      ┌───────────────────────────┐
│  Google Sheets Database   │      │    Notification Layer     │
│       (GymFlow_DB)        │      │  • Telegram Bot (Active)  │
│  • Members Tab            │      │  • WhatsApp Cloud API     │
│  • Payments Tab           │      │    (Upgrade Ready)        │
└───────────────────────────┘      └───────────────────────────┘
```

---

## ⚙️ Automated Workflows

| Workflow ID | Name | Trigger | Core Functionality | Documentation | Export |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **WF-01** | **Member Registration** | n8n Form Trigger | Captures new sign-ups, validates plan (`Daily`, `Weekly`, `Monthly`), generates unique `Member_ID`, computes `Expiry_Date`, and appends to `Members`. | [Guide](docs/workflows/WF-01-member-registration.md) | [JSON](WorkFlows/WF-01%20-%20Member%20Registration.json) |
| **WF-02** | **Expiry Reminders** | Daily Schedule (9:00 AM IST) | Scans active memberships and sends automated Telegram alerts at **7, 3, 1, and 0 days** remaining while ignoring inactive records. | [Guide](docs/workflows/WF-02-expiry-reminders.md) | [JSON](WorkFlows/WF-02%20-%20Expiry%20Reminders.json) |
| **WF-03** | **Payment Management** | n8n Form Trigger | Looks up member by `Member_ID`, updates `Payment_Status` to `Paid`, logs an immutable transaction record in `Payments`, and sends a Telegram receipt. | [Guide](docs/workflows/WF-03-payment-management.md) | [JSON](WorkFlows/WF-03%20-%20Payment%20Management.json) |
| **WF-04** | **Daily Reports** | Daily Schedule (9:00 AM IST) | Runs parallel branches across `Members` and `Payments`, merges metrics, and delivers a morning summary of active/expired counts and daily revenue. | [Guide](docs/workflows/WF-04-daily-reports.md) | [JSON](WorkFlows/WF-04%20-%20Daily%20Reports.json) |

---

## 🗄️ Database Schema (`GymFlow_DB`)

GymFlow uses a two-tab Google Sheets structure to separate current member state from historical payment transactions:

### 1. `Members` Tab
| Column | Example | Description |
| :--- | :--- | :--- |
| `Member_ID` | `GYM-1795006618399-252` | Auto-generated unique identifier |
| `Name` | `Rahul Sharma` | Member's full name |
| `Phone` | `9876543210` | Contact number (stored as string) |
| `Join_Date` | `2026-11-18` | Membership start date (`YYYY-MM-DD`) |
| `Plan` | `Monthly` | `Daily` (1d), `Weekly` (7d), or `Monthly` (30d) |
| `Expiry_Date` | `2026-12-17` | Calculated end date (`YYYY-MM-DD`) |
| `Payment_Status` | `Paid` | `Pending` on registration; updated to `Paid` by WF-03 |
| `Membership_Status` | `Active` | `Active` or `Inactive` |
| `Registered_At` | `2026-11-18T18:20:24+05:30` | ISO-8601 registration timestamp (IST) |

### 2. `Payments` Tab
| Column | Example | Description |
| :--- | :--- | :--- |
| `Payment_ID` | `PAY-1795031495757` | Auto-generated transaction ID |
| `Member_ID` | `GYM-1795006618399-252` | Foreign key linking to `Members` |
| `Amount` | `1500` | Amount collected (INR) |
| `Payment_Date` | `2026-11-21` | Date of payment (`YYYY-MM-DD`) |
| `Payment_Method` | `UPI` | `Cash`, `UPI`, `Bank Transfer`, or `Card` |
| `Transaction_Reference` | `UPI-9981201` | Optional UTR / receipt reference |
| `Recorded_At` | `2026-11-21T08:15:00+05:30` | ISO-8601 audit timestamp (IST) |

---

## 🛠️ Tech Stack

* **Workflow Orchestration:** n8n Community Edition (Self-Hosted)
* **Containerization:** Docker & Docker Compose (with persistent volume `gymflow_data`)
* **Database:** Google Sheets API (OAuth2)
* **Business Logic:** JavaScript (n8n Code Nodes for IST timezone handling, date diffs, and aggregation)
* **Notifications & Webhooks:** Telegram Bot API + ngrok HTTPS Tunnel

---

## 📂 Repository Structure

```text
GymFlow/
├── .gitignore
├── README.md
├── docker-compose.yml
├── docs/
│   ├── project-overview.md
│   ├── setup-guide.md
│   └── workflows/
│       ├── WF-01-member-registration.md
│       ├── WF-02-expiry-reminders.md
│       ├── WF-03-payment-management.md
│       └── WF-04-daily-reports.md
└── WorkFlows/
    ├── WF-01 - Member Registration.json
    ├── WF-02 - Expiry Reminders.json
    ├── WF-03 - Payment Management.json
    └── WF-04 - Daily Reports.json
```

---

## 🚀 Quick Start

1. **Start the local n8n container:**
   ```powershell
   docker compose up -d
   ```
2. **Access the n8n editor:** Open `http://localhost:5678` in your browser.
3. **Import workflows:** Import the JSON files from `WorkFlows/` into n8n and attach your **Google Sheets OAuth2** and **Telegram Bot** credentials.
4. **Configure database:** Create a Google Spreadsheet named `GymFlow_DB` with `Members` and `Payments` tabs matching the schema above.

For detailed shutdown, startup, and webhook instructions, see the [Local Setup Guide](docs/setup-guide.md) and [Project Overview](docs/project-overview.md).

---

## 🔮 Roadmap

* [ ] **WhatsApp Business Cloud API:** Direct renewal reminders and payment receipts to members' WhatsApp numbers.
* [ ] **Duplicate Reminder Guard (`Reminder_Logs`):** Log sent alerts to prevent duplicate notifications if workflows re-run.
* [ ] **Partial Payment & Renewal Extensions:** Automatically extend `Expiry_Date` when an existing member renews their plan.
* [ ] **Cloud VPS Deployment:** Host n8n on a 24/7 cloud instance for commercial gym onboarding.