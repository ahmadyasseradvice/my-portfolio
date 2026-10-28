# Brevo integration spec

- Endpoint: `POST https://api.brevo.com/v3/smtp/email`. Header `api-key: $BREVO_API_KEY`.
- Sender must be the verified Brevo email (`BREVO_SENDER_EMAIL`).
- `client.ts`: a fetch wrapper with error handling and timeout.
- `sendEmail.ts`: `sendEmail({to, subject, html, replyTo?, attachments?})`.
- `templates.ts`: HTML for contact notification, contact auto-reply, booking confirmation, booking notification.

## Contact flow
Form → client Zod check → `/api/contact` → server Zod check + honeypot + rate limit → Brevo (notify owner, auto-reply) → JSON `{ok}` → UI success or error toast.

## Calendar flow
Pick a date → `GET /slots` → pick a slot → details form → `POST /book` → re-check the slot → store in the DB adapter → confirmation emails with an `.ics` file.

## Future
Optional: save contacts to a Brevo list via `/v3/contacts`. Use `DATABASE_URL` with Prisma for persistent bookings.
