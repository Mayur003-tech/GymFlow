# WF-02 - Expiry Reminders

## Overview
Runs daily at 9:00 AM IST, reads active members from GymFlow_DB -> Members, identifies memberships with 7, 3, 1, or 0 days remaining, and sends personalized Telegram alerts.

## Flow
Schedule Trigger (9:00 AM) -> Google Sheets (Get Rows) -> Check Expiry Dates (Code) -> Telegram (Send Text Message)
