# WF-01 - Member Registration

## Overview
Captures new gym member sign-ups via an n8n Form Trigger, generates a unique Member_ID, calculates Expiry_Date based on the selected plan (Daily = 1 day, Weekly = 7 days, Monthly = 30 days), and appends the record to GymFlow_DB -> Members.

## Flow
n8n Form Trigger -> Process Member Data (Code) -> Google Sheets (Append Row)
