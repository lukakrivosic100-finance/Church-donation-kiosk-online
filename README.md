# Church Donation Kiosk — Cash Only

The iPad donation kiosk now issues **real, unique cash-donation reference numbers** using a Supabase database. No card, Gift Aid or simulated payment function is present.

## How a real donation works

1. A visitor selects English or Serbian, then chooses an amount (between **£1 and £499**).
2. The kiosk requests a cash-reference ticket from the server. The first number is **060**; later references increase in sequence.
3. A saved ticket is **AWAITING CASH — NOT PAID**. The visitor tells or shows the number to a staff member at the church desk and hands over the money.
4. The staff member opens **cash-desk.html**, signs in with their existing authorised Church Debt Account staff username and PIN, looks up the number, checks the amount, **counts and receives the actual cash**, then clicks **Cash received — Mark Paid**.
5. The server stores the confirmation timestamp and staff account ID. An already-paid or void reference cannot be marked paid again.
6. The kiosk has a **Next customer** button and automatically returns to language selection after two minutes on the reference screen.

Do not tell a donor a ticket is paid simply because a number was issued. A cash reference is **not a payment receipt**.

## Church cash desk

- Open [Cash Desk](./cash-desk.html) on an authorised device.
- Existing staff accounts from the separately maintained Church Debt Account system are used for authentication. Staff usernames and PINs are **never stored in kiosk HTML**.
- The staff page shows the latest 150 ticket records, with a limited CSV export of those displayed records.
- Cash donation tickets are **separate from** customers' church debt balances and do not modify debt transactions.
- For complete records, authorised church administrators should use the database or a properly managed accounting export and reconcile donations with physically received cash.

## Backend

Supabase project: `acyjdlrsvtgqqazzxzys`

Edge Function: `church-cash-donations`

Table: `public.church_cash_donations` with row-level security and no public direct table access. Public visitors can only request a reference; staff session verification is required to search and mark references paid or void.

The public donation API is subject to approximate anti-abuse rate limiting. If it fails, **no reference should be presented as issued**.

## Before using it for real church funds

- Confirm that GitHub Pages is enabled and serves the newest `index.html` and `cash-desk.html`.
- Test the entire process on the actual kiosk and staff device: requesting a reference, retrieving it on the desk, confirming cash, and seeing it as paid. Do not mark a ticket paid without actually receiving cash.
- Confirm church staff accounts and network access, and establish the cash-handling, reconciliation, retention and receipt policies approved by the church.
- Verify that the church desk has a reliable internet connection. Reference creation and staff confirmation require the Supabase service.
- Only authorised staff should have access to cash records; all staff sign-ins are short-lived.

This setup records *staff-confirmed cash reception*. It does **not** count physical notes/coins or automatically verify cash has been handed over.
