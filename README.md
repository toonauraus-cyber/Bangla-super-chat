# Bangla Super Chat — starter project

This is a deployable foundation for a creator-support platform with:
- Creator and shop registration/login
- PostgreSQL user, transaction and settings records
- Admin dashboard at `/admin`
- Live admin notifications for new accounts and transactions (Socket.IO)
- Admin approve/reject/cancel for pending transactions
- Withdrawal request history
- Admin account blocking/unblocking and basic platform settings

## Important scope and payment safety

This starter is **not yet a complete live donation/payment gateway**. bKash/Nagad/Rocket/Upay collection and automated payouts require approved merchant accounts, provider API credentials, callback/webhook verification, reconciliation and testing. Do not collect customer money by treating a submitted transaction ID as proof of payment. The current withdrawal request is an approval workflow; it does not send money automatically. A withdrawal approval debits the internal wallet only if sufficient balance exists. No public API in this project creates wallet credit; verified donation/deposit processing must be implemented with the chosen provider before launch.

## Run locally

1. Install Node.js 20+ and PostgreSQL.
2. Create a database and run `schema.sql` against it.
3. Copy `.env.example` to `.env` and set values.
4. Generate a bcrypt hash for the initial admin password:

   ```bash
   node -e "require('bcryptjs').hash('REPLACE_WITH_A_LONG_UNIQUE_PASSWORD',12).then(console.log)"
   ```

   Put the resulting hash in `ADMIN_PASSWORD_HASH`, and your admin email in `ADMIN_EMAIL`.
5. Install and start:

   ```bash
   npm install
   npm start
   ```

6. Open `http://localhost:3000` and `http://localhost:3000/admin`.

The initial admin account is created on first boot if the configured email is not already present. The email in `ADMIN_EMAIL` is assigned the admin role. Keep the `.env` secret out of GitHub.

## GitHub

Create a new empty GitHub repository named `bangla-super-chat`, then upload the project files. Do not upload `.env`, database credentials, session secrets or real payment API keys. `.gitignore` is included.

## Render deployment

1. Push this project to GitHub.
2. In Render, create a **PostgreSQL** database.
3. Create a **Web Service** connected to the repository.
4. Build command: `npm install`
5. Start command: `npm start`
6. Add environment variables from `.env.example` in Render. Set `NODE_ENV=production`, `DATABASE_URL` to the Render PostgreSQL internal connection string, `SESSION_SECRET` to a long random secret, `ADMIN_EMAIL`, and `ADMIN_PASSWORD_HASH`.
7. Run `schema.sql` once against the Render database using Render's database connection tools.
8. Deploy and test registration, admin login, account listing, withdrawal request, and admin decisions.

## Before public launch

Add CSRF protection, rate limiting, email verification, password reset, audit logs, stronger input validation, privacy/terms pages, backups, monitoring, admin 2FA, and provider-specific verified payment webhooks. Review local legal, tax, consumer protection and payment-provider requirements. Use HTTPS only.
