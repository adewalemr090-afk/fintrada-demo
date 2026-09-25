# FINTRADA — Demo Platform

**Trade • Invest • Grow**

This package is a working **demo/prototype** of the FINTRADA platform. It includes:

- Responsive FINTRADA landing page
- Simulated market feed
- User registration and login
- Demo-mode automatic email verification
- NGN and USD wallets
- Demo wallet balances
- Investment opportunities across Agriculture, Transportation, Education and Technology
- Deposit requests + admin approval/rejection
- Withdrawal requests + admin approval/rejection
- Admin dashboard
- Treasury ledger
- Audit log
- Health check for deployment

## Important

This is a demonstration environment. It does **not** move real money and the market prices are synthetic. The investment UI uses **target: up to 10% annually; not guaranteed**.

Before accepting public funds or launching investment products in Nigeria, obtain qualified legal/regulatory advice and implement the required KYC/AML, payment, custody, accounting, security, disclosure and licensing controls.

## 1. Run locally

Requirements: Node.js 20+.

```bash
cd FINTRADA
copy .env.example .env
npm install
npm start
```

Then open:

`http://localhost:3000`

For the easiest local demo, keep `DEMO_MODE=true` in `.env`.

### Demo user

- Email: `demo@fintrada.test`
- Password: `Demo_12345!`
- Starting demo balance: NGN 1,000,000 and USD 1,000

### Admin

The admin credentials come from `ADMIN_EMAIL` and `ADMIN_PASSWORD` in `.env`.

## 2. Deploy a free demo on Render

Render supports Node.js web services and can deploy from a connected GitHub repository. A free web service is suitable for testing, but it spins down after inactivity and its local filesystem is ephemeral. That means this SQLite demo can reset its local database after a restart/spin-down. For a persistent application, move the database to managed PostgreSQL.

### Step A — Upload this folder to GitHub

Create a new GitHub repository, for example:

`fintrada-demo`

Upload the **contents of this FINTRADA folder** so that `package.json`, `server.js`, `render.yaml` and `public/` are at the repository root.

Do not upload `.env` or real passwords.

### Step B — Create the Render service

1. Open Render and sign in.
2. Choose **New → Web Service**.
3. Connect your GitHub account and select the `fintrada-demo` repository.
4. Render can read `render.yaml`, or you can enter the settings manually.
5. Use:
   - Runtime: Node
   - Build command: `npm install`
   - Start command: `npm start`
   - Health check: `/api/health`
6. Create the service.

### Step C — Set admin credentials

In Render → your service → **Environment**, set:

- `ADMIN_EMAIL` = your private admin email
- `ADMIN_PASSWORD` = a strong private password
- `DEMO_MODE` = `true`
- `DEMO_EMAIL` = `demo@fintrada.test`
- `DEMO_PASSWORD` = `Demo_12345!`

Render automatically generates `JWT_SECRET` from the Blueprint. Keep secrets in Render Environment Variables; do not commit them to GitHub.

After deployment, Render gives you an `onrender.com` URL. Set `PUBLIC_URL` to that exact HTTPS URL and redeploy if you want verification links to use the public address.

## 3. Demo test sequence

1. Open the Render URL.
2. Click **Login**.
3. Sign in with the demo user above.
4. Confirm the NGN and USD demo wallets.
5. Open **Request deposit** and create a pending deposit.
6. Log out.
7. Log in with the admin account.
8. Open **Admin console**.
9. Approve the deposit and verify the wallet is credited.
10. Log back into the demo user.
11. Start an investment using the credited wallet balance.
12. Submit a withdrawal.
13. Return to the admin console and approve/reject the withdrawal.
14. Check the audit log.

## 4. Next production build

For a real launch, the next engineering stage should replace SQLite with PostgreSQL and introduce a proper double-entry ledger, payment-provider webhooks, KYC/AML, 2FA, granular admin roles, reconciliation, backups, monitoring, tested investment calculations, real market-data providers, secure custody/payment architecture and professional legal/regulatory review.
