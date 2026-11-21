# WF-04 - Daily Reports

## Overview
Aggregates total, active, expired, and expiring-soon member counts alongside daily payment count and revenue at 9:00 AM IST, sending a formatted daily summary to Telegram.

## Flow
Schedule Trigger -> [Get Members -> Calculate Stats] + [Get Payments -> Calculate Revenue] -> Merge (Combine by Position) -> Telegram (Daily Report)
