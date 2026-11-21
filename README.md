# GymFlow - Gym Membership Automation

Automated gym membership management built on self-hosted **n8n**, **Google Sheets**, and **Telegram**.

## Workflows
| ID | Documentation | n8n Export | Trigger |
|---|---|---|---|
| **WF-01** | [Member Registration](docs/workflows/WF-01-member-registration.md) | [JSON](WorkFlows/WF-01%20-%20Member%20Registration.json) | n8n Form |
| **WF-02** | [Expiry Reminders](docs/workflows/WF-02-expiry-reminders.md) | [JSON](WorkFlows/WF-02%20-%20Expiry%20Reminders.json) | Daily Cron (9 AM) |
| **WF-03** | [Payment Management](docs/workflows/WF-03-payment-management.md) | [JSON](WorkFlows/WF-03%20-%20Payment%20Management.json) | n8n Form |
| **WF-04** | [Daily Reports](docs/workflows/WF-04-daily-reports.md) | [JSON](WorkFlows/WF-04%20-%20Daily%20Reports.json) | Daily Cron (9 AM) |

## Setup
See [Local Setup Guide](docs/setup-guide.md) and [Project Overview](docs/project-overview.md).
