# Foundation School Form Engine Setup

This package writes website registrations to Google Sheets first, then attempts Mailchimp subscription as a non-blocking step.

## Mailchimp Setup (SETTINGS tab)

Run `RUN_ME_FIRST_Setup_Sheets` (or `setupStarterSheets`) in Apps Script once.  
This creates or updates a `SETTINGS` tab with:

- `MAILCHIMP_ENABLED` (`TRUE` or `FALSE`)
- `MAILCHIMP_API_KEY`
- `MAILCHIMP_AUDIENCE_ID`
- `MAILCHIMP_SERVER_PREFIX` (example: `us21`)
- `MAILCHIMP_STATUS` (example: `subscribed` or `pending`)

If `MAILCHIMP_ENABLED` is not `TRUE`, Mailchimp is skipped safely.

## Where To Find Mailchimp Values

1. API key:
- Mailchimp -> Account -> Extras -> API keys

2. Audience/List ID:
- Mailchimp -> Audience -> Settings -> Audience name and defaults  
- Copy the Audience ID

3. Server prefix:
- Usually the suffix in your API key (for example `...-us21` means `us21`)
- Or from your Mailchimp account/API docs region info

## Runtime Behavior

1. `doPost` saves to the `APPLICANTS` sheet first.
2. It then calls Mailchimp using API v3.0.
3. Mailchimp failures are logged and returned as `mailchimpReason`, but do not fail the form submission.
4. Duplicate-safe logic uses Mailchimp member upsert (existing email is updated instead of failing).

## Intake Mode (De-stale Guard)

- `REGISTRATION_SOURCE` is set in `CONFIG`.
- Default is `WEBSITE` (recommended for this package).
- When `REGISTRATION_SOURCE=WEBSITE`, legacy Phase 2 Google Form ingestion is skipped by guard.
- Set `REGISTRATION_SOURCE=GOOGLE_FORM` only if you intentionally want legacy FormApp ingestion.
# foundation-school-site
