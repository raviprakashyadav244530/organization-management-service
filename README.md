# The Wedding Company — Full-Stack Assessment  
Frontend (React + TypeScript) · Backend (Node.js + TypeScript + MongoDB)

[![CI](https://img.shields.io/badge/CI-GitHub%20Actions-blue)](./.github/workflows/ci.yml) ![License](https://img.shields.io/badge/License-MIT-green) ![Stack](https://img.shields.io/badge/Stack-React%20%7C%20Node%20%7C%20MongoDB%20%7C%20TypeScript-purple)

## Overview
- Admin: create organizations, authenticate, update/rename, and delete orgs with JWT auth.
- Dynamic org collections: `org_<sanitized_name>` per organization with secure bcrypt hashing.
- Booking UI: browse shows/trips, select seats on a grid, manage state via React Context.
- Accessible, keyboard-operable UI with simple Tailwind styling.
- Tests: backend unit/integration + concurrency using mongodb-memory-server; frontend build verified.

## Architecture
```
                               +----------------------+
                               |      Frontend        |
                               | React + TS + Vite    |
                               | Routes: / /admin /booking/:id
                               +----------+-----------+
                                          |
                        HTTPS (VITE_API_BASE_URL -> Render/Railway)
                                          |
                               +----------v-----------+
                               |      Backend         |
                               | Node + Express + TS  |
                               | Routes: /org/* /admin/login /health
                               +----------+-----------+
                                          |
                               +----------v-----------+
                               |     MongoDB Atlas    |
                               | master.organizations |
                               | master.admins        |
                               | org_<name> collections
                               +----------------------+
```

## API Summary
- `POST /org/create` – create org, admin user, dynamic collection.
- `GET /org/get?organization_name=` – fetch org metadata.
- `PUT /org/update` – rename org, sync collection, update admin (JWT).
- `DELETE /org/delete` – delete org & collection (JWT).
- `POST /admin/login` – admin auth, returns JWT.
- `GET /health` – service health check.

## Deploy Links (placeholders)
- Frontend (Vercel): _https://your-frontend.vercel.app_
- Backend (Render/Railway): _https://your-backend.onrender.com/health_
- Swagger/Postman: `backend/docs/api-postman.json` (add after export)

## Screenshots
Add images to `frontend/docs/screenshots/`:
- `screenshot_home.png`
- `screenshot_admin_page.png`
- `screenshot_booking_page.png`
- `screenshot_network_booking.png`
- `screenshot_backend_logs.png`
- `screenshot_render_deploy.png`
- `screenshot_vercel_deploy.png`
- `screenshot_ci_pass.png`

## Setup
### Prereqs
- Node.js 18+ (tested on 22)
- npm
- MongoDB (local or Atlas) for backend runtime (tests use in-memory)

### Backend
```bash
cd backend
cp env.example .env   # set MONGO_URI, JWT_SECRET, PORT, SALT_ROUNDS
npm install
npm run dev           # local dev
npm test              # run unit/integration/concurrency tests
npm run build && npm start
```

### Frontend
```bash
cd frontend
npm install
npm run dev           # http://localhost:5173
npm run build         # production build
```
- Configure `VITE_API_BASE_URL` in `.env` (e.g., backend base URL).

## Testing
- Backend: `npm test` (Jest + supertest + mongodb-memory-server).
- Frontend: `npm run build` (ensures type-safety and bundling); add RTL tests as needed.
- CI: see `.github/workflows/ci.yml`.

## Deployment
- Backend (Render/Railway):
  - Build: `npm ci && npm run build`
  - Start: `npm start`
  - Env: `MONGO_URI`, `JWT_SECRET`, `PORT`, `SALT_ROUNDS`, `NODE_ENV=production`
  - Expose `/health`
- Frontend (Vercel/Netlify):
  - Build: `npm run build`
  - Output: `dist`
  - Env: `VITE_API_BASE_URL`
See `docs/deployment.md` for detailed steps.

## Technologies
- Frontend: React, TypeScript, Vite, Tailwind (modern @import style), React Router, Axios.
- Backend: Node.js, Express, TypeScript, MongoDB/Mongoose, JWT, bcrypt, Zod, Jest, Supertest.
- Tooling: GitHub Actions, mongodb-memory-server, ts-node-dev, Helmet/CORS/Morgan.

## Repository Structure
```
.
├─ backend/          # Express + TS service, tests, configs
├─ frontend/         # React + TS client, routes, components
├─ docs/             # system design, deployment, testing, api docs, assumptions
├─ scripts/          # helper scripts (placeholder)
├─ .github/workflows/ci.yml
├─ README.md
├─ submission.md
└─ LICENSE
```

## Author & Notes
- Built for The Wedding Company full-stack assessment.
- Feel free to extend with additional E2E tests or visual regression checks.

## Badges Legend
- CI badge reflects `.github/workflows/ci.yml` status.
- License: MIT.

