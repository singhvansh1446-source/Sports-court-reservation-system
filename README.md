# Sports Court Reservation System (SCRS)

A Next.js 16 + TypeScript + Prisma web app that lets users explore sports, browse facilities, register, log in and book courts.

## Prerequisites

- **Node.js** 20.9+ (Node.js 22 LTS recommended; required by Next.js 16) — https://nodejs.org
- **Bun** (optional, faster installs) — https://bun.sh

## Setup (Windows / VS Code)

1. Install **Node.js 22 LTS** from https://nodejs.org (Next.js 16 needs Node.js 20.9 or newer).
2. Extract this ZIP and open the `SCRS-VSCode-fixed` folder in VS Code.
3. In VS Code, open **Terminal → New Terminal** and run:

```powershell
npm install
npm run db:generate
npm run db:push
npm run dev
```

4. Open http://localhost:3000 in your browser.

The project includes a SQLite database at `db/custom.db`. The `.env` file now uses a project-relative database path, rather than the original Linux-only absolute path.

If you want to reset the database and load the demo data, run `npm run db:seed`. **Warning:** this clears existing users, bookings, courts, facilities and sports before inserting demo data. Do not run it if you need to keep existing records.

## Demo account

- Email: `demo@scrs.app`
- Password: `demo1234`

You can also register a brand-new account from the Sign In dialog.

## Tech stack

- Next.js 16 (App Router) + TypeScript
- Tailwind CSS 4 + shadcn/ui components
- Prisma ORM + SQLite (file-based, lives in `db/custom.db`)
- Zustand for client state
- Cookie-based signed session auth (pbkdf2 password hashing)

## Project structure

```
prisma/schema.prisma     # User, Sport, Facility, Court, Booking models
scripts/seed.ts          # demo data seeder
src/app/api/             # REST endpoints (auth, sports, facilities, courts, bookings)
src/app/page.tsx         # main page shell with view switching
src/components/scrs/     # app UI (navbar, auth-dialog, booking-dialog, views)
src/components/ui/        # shadcn/ui primitives
src/lib/auth.ts          # session + password hashing
src/lib/db.ts            # Prisma client singleton
src/store/app.ts         # Zustand store
src/types/index.ts       # shared client-side domain types
```

## Useful scripts

| Command | What it does |
|---|---|
| `bun run dev` | Start the dev server on port 3000 |
| `bun run lint` | Run ESLint |
| `bun run db:push` | Push schema changes to the SQLite file |
| `bun run scripts/seed.ts` | Re-seed demo data (wipes bookings/courts/facilities/sports/users first) |

## Troubleshooting

| Problem | Fix |
|---|---|
| `Cannot find module '@prisma/client'` | Run `bun run db:generate` (or `npx prisma generate`) |
| Port 3000 already in use | Kill the other process, or run `npx next dev -p 3001` |
| Database locked / stale | Delete `db/custom.db` and re-run `db:push` + seed |
| Tailwind classes not styling | Restart the dev server; make sure `postcss.config.mjs` and `tailwind.config.ts` exist |

## Re-seed anytime

If you want a clean slate while developing:

```bash
bun run scripts/seed.ts
```

This wipes all existing data and re-inserts the demo user, sports, facilities and courts.


**Important:** `npm run db:seed` clears existing bookings, courts, facilities, sports and users before adding demo data. Use it only for a fresh demo database.
