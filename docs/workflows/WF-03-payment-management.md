# WF-03 - Payment Management

## Overview
Processes membership payments submitted via form, looks up the member by Member_ID, updates Payment_Status to Paid in Members, logs the transaction in Payments, and sends a Telegram confirmation.

## Flow
Payment Form Trigger -> Google Sheets (Get Row) -> Google Sheets (Update Row) -> Google Sheets (Append Row) -> Telegram (Confirmation)
