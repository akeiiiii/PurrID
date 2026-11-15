# PurrID

**A Smart Stray Animal Identification and Monitoring System for Barangay Tinurik, Tanauan City, Batangas.**

PurrID is a web-based system for managing stray-animal records, monitoring reports, donations, adoption requests, accounts, and public animal information.

## Requirements

- Node.js **22.5 or later** (the application uses the built-in `node:sqlite` module)
- No third-party npm packages are currently required.

## Run locally

1. Clone or download this repository.
2. Open a terminal in the project folder.
3. Start the server:

   ```bash
   node server.js
   ```

4. Open <http://localhost:3000> in your browser.

The SQLite database (`purrid.db`) is created automatically on first run. It is intentionally excluded from Git because it can contain account and community data. Keep backups of your own database somewhere private.

## Administrator account

On a fresh database, the application creates an initial administrator account:

- Username: `admin`
- Initial password: `admin123`

**Change the initial password before using the system with real data.** Set `ADMIN_PASSWORD` before the first run, for example:

```bash
ADMIN_PASSWORD='your-strong-password' node server.js
```

The environment variable is only used when the initial administrator is created. Changing it later does not change an existing administrator password.

## Configuration

- `PORT` — server port (defaults to `3000`)
- `DB_FILE` — SQLite database file path (defaults to `./purrid.db`)
- `ADMIN_PASSWORD` — initial administrator password for a newly created database

## Deployment

A Render service configuration is provided in `render.yaml`. Configure `ADMIN_PASSWORD` as a secret environment variable in the hosting dashboard. Use persistent storage for the SQLite database in production; an ephemeral filesystem can lose database changes when the service restarts or redeploys.

## DFD process-to-code map

| DFD process | Implementation |
|---|---|
| 1.0 Manage Accounts (1.1–1.4) | `POST /api/register`, `POST /api/login`, admin Accounts tab |
| 2.0 Register Stray Animals (2.1–2.4) | `POST /api/animals`, Admin Animal Records tab |
| 3.0 Monitor Animals (3.1–3.4) | `POST /api/monitoring`, `PUT /api/admin/monitoring/:id`, user Dashboard |
| 4.0 Search Animal Records (4.1–4.3) | `GET /api/animals?q=` searches animal and monitoring information |
| 5.0 Manage Donations (5.1–5.4) | `POST /api/donations` |
| 6.0 Process Adoptions (6.1–6.5) | `POST /api/adoptions`, `PUT /api/admin/adoptions/:id` |
| 7.0 Generate Reports and Information (7.1–7.4) | `/api/admin/system-reports`, `GET /api/public`, public animal pages |

## Database tables

`admins`, `users`, `adopters`, `stray_animals`, `monitoring_reports`, `adoptions`, `donations`, `system_reports`, and `donation_audits`.

## License

This project is currently marked `UNLICENSED` in `package.json`. Do not assume it is open-source or reuse it under an open-source license without the project owner's permission.
