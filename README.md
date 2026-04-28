# Liquid DMS — Admin Panel

> Desktop-first admin panel for a yacht-rental document management system. Next.js (App Router) front end, an embedded Express/Mongoose API, and native Windows packaging via **Tauri**.

![Stack](https://img.shields.io/badge/next.js-14-black)
![Desktop](https://img.shields.io/badge/desktop-tauri-orange)
![API](https://img.shields.io/badge/api-express-green)

## Overview

This repo is the **operator/admin side** of the Liquid DMS product (internal codenames *Faraway* / *SailAway*). It lets an operator manage their fleet and paperwork from a single desktop application:

- **Authentication** — operator login and session management (JWT + bcrypt).
- **Dashboard** — fleet and booking KPIs with charts.
- **Yachts** — listing management with rich-text editors (TipTap / CKEditor / Editor.js) for descriptions and policies.
- **Media** — image/document uploads to S3 and Cloudinary (multer + `@aws-sdk`).
- **Settings** — profile and application configuration.

The `backend/` directory contains an Express MVC service (Mongoose models, controllers, routers, validation, middleware) that the panel talks to, so the whole system can run locally as a self-contained desktop app.

## Architecture

```
src/                 Next.js App Router front end
  app/(auth)         login / session screens
  app/(root)         dashboard, yachts, settings
  components/        Dashboard, Yachts, SailAwayCard, Sidebar, Header, Drawer, ...
  lib, common, data, types
backend/             Express API (models, controllers, router, middleware, validations)
src-tauri/           Tauri (Rust) native shell -> Windows installer
tools/voice/         local TTS/STT utilities
```

The app began as a web admin and was packaged for desktop: first with **Electron**, then migrated to **Tauri** for a small installer and low memory footprint.

## Getting started

```bash
npm ci              # install dependencies
npm run dev         # Next.js dev server on http://localhost:3001
```

Backend (separate service):

```bash
cd backend && npm ci && npm start
```

### Desktop (Tauri)

Prerequisites (Windows): Rust toolchain (MSVC) from https://rustup.rs and Visual Studio Build Tools (“Desktop development with C++”).

```bash
npm run dev:tauri      # Next.js + Tauri window in development
npm run build          # production Next.js build
npm run build:tauri    # build Windows installer/bundle via Tauri
```

By default the shell loads `http://localhost:3001`. For production either use a static export pointed at local files, or run a sidecar server and point the Tauri window at `http://127.0.0.1:<port>`.

## Tech stack

| Layer | Choice |
| --- | --- |
| Framework | Next.js (App Router), React |
| State | Redux Toolkit |
| Styling | Tailwind CSS, shadcn/ui, Radix, lucide-react |
| Editors | TipTap, CKEditor 5, Editor.js |
| API | Express, Mongoose, JWT, express-validator, helmet, rate-limit |
| Storage | AWS S3, Cloudinary, multer |
| Charts | Recharts |
| Desktop | Tauri (Rust) |

## License

© [Jahanzaib Iftikhar](https://github.com/jahanzaib0013-a11y)
