# Kisan Cold Chain

AI-powered post-harvest cold chain management platform for Indian farmers. Connects farmers with cold storage, transport pooling, spoilage prediction, WhatsApp booking, and role-based dashboards for operators, transport providers, and admins.

---

## Tech Stack

### Monorepo & Tooling

| Layer | Technology |
|-------|------------|
| Package manager | **pnpm** workspaces |
| Runtime | **Node.js 24** |
| Language | **TypeScript 5.9** |
| Build (API) | **esbuild** (CJS bundle) |
| Build (Web) | **Vite 7** |
| Lint / format | **Prettier** |

### Frontend (`artifacts/kisan-cold-chain`)

| Layer | Technology |
|-------|------------|
| Framework | **React 19** |
| Routing | **Wouter** |
| Server state | **TanStack React Query v5** |
| Styling | **Tailwind CSS v4** (`@tailwindcss/vite`) |
| UI components | **Radix UI** + **shadcn/ui**-style components |
| Icons | **Lucide React**, **React Icons** |
| Forms | **React Hook Form** + **Zod** resolvers |
| Charts | **Recharts** |
| Animation | **Framer Motion** |
| Notifications | **Sonner**, Radix Toast |
| Theming | **next-themes** |
| API client | `@workspace/api-client-react` (generated) |

### Backend (`artifacts/api-server`)

| Layer | Technology |
|-------|------------|
| Framework | **Express 5** |
| Logging | **Pino** + **pino-http** |
| Middleware | **CORS**, **cookie-parser** |
| Validation | **Zod** (`@workspace/api-zod`) |

### Database (`lib/db`)

| Layer | Technology |
|-------|------------|
| Database | **PostgreSQL 16** |
| ORM | **Drizzle ORM** |
| Schema validation | **drizzle-zod** |
| Migrations | **drizzle-kit** (`push`) |
| Driver | **pg** (node-postgres) |

### API Contract & Codegen

| Layer | Technology |
|-------|------------|
| API spec | **OpenAPI 3.1** (`lib/api-spec/openapi.yaml`) |
| Codegen | **Orval** → React Query hooks + Zod schemas |
| Output packages | `@workspace/api-client-react`, `@workspace/api-zod` |

### Integrations & Features

| Feature | Technology / Approach |
|---------|----------------------|
| AI spoilage prediction | Gemini AI (via API routes) |
| WhatsApp booking | WhatsApp webhook routes |
| Automation | n8n workflow blueprints |
| Weather risk | Weather API routes + DB schema |
| Auth | Client-side role-based auth context (farmer, operator, transport, admin) |

### Deployment (Replit)

| Layer | Technology |
|-------|------------|
| Platform | Replit (PNPM workspace stack) |
| Web port | `24832` |
| API port | `8080` |
| Mockup sandbox port | `8081` |
| Plugins | `@replit/vite-plugin-cartographer`, `dev-banner`, `runtime-error-modal` |

---

## Folder Structure

```
Kisan-Cold-Chain/
├── .replit                          # Replit project config (ports, workflows)
├── .replitignore
├── package.json                     # Root workspace scripts (build, typecheck)
├── pnpm-workspace.yaml              # Workspace + dependency catalog
├── pnpm-lock.yaml
├── replit.md                        # Internal run/operate notes
│
├── attached_assets/                 # Design notes and reference assets
│   └── Pasted--Required-Changes-Home-Page-*.txt
│
├── artifacts/                       # Deployable applications
│   │
│   ├── kisan-cold-chain/            # Main React web app (Vite)
│   │   ├── .replit-artifact/
│   │   │   └── artifact.toml        # Web service config (PORT=24832)
│   │   ├── public/
│   │   │   ├── favicon.svg
│   │   │   └── robots.txt
│   │   ├── src/
│   │   │   ├── App.tsx              # Routes + auth guards
│   │   │   ├── main.tsx
│   │   │   ├── index.css            # Tailwind + theme tokens
│   │   │   ├── components/
│   │   │   │   ├── layout.tsx       # App shell / navigation
│   │   │   │   └── ui/              # 55 shadcn/Radix UI components
│   │   │   ├── contexts/
│   │   │   │   └── auth.tsx         # Auth + role-based access
│   │   │   ├── hooks/
│   │   │   │   ├── use-mobile.tsx
│   │   │   │   └── use-toast.ts
│   │   │   ├── lib/
│   │   │   │   └── utils.ts
│   │   │   └── pages/
│   │   │       ├── home.tsx         # Landing page
│   │   │       ├── login.tsx
│   │   │       ├── signup.tsx
│   │   │       ├── access-denied.tsx
│   │   │       ├── not-found.tsx
│   │   │       ├── spoilage.tsx     # AI spoilage prediction
│   │   │       ├── cold-stores.tsx  # Cold store discovery
│   │   │       ├── transport.tsx
│   │   │       ├── transport-dashboard.tsx
│   │   │       ├── whatsapp.tsx
│   │   │       ├── notifications.tsx
│   │   │       ├── n8n.tsx
│   │   │       ├── farmer/
│   │   │       │   ├── dashboard.tsx
│   │   │       │   ├── harvests.tsx
│   │   │       │   ├── new-harvest.tsx
│   │   │       │   └── bookings.tsx
│   │   │       ├── operator/
│   │   │       │   └── dashboard.tsx
│   │   │       └── admin/
│   │   │           ├── dashboard.tsx
│   │   │           └── analytics.tsx
│   │   ├── vite.config.ts
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   ├── api-server/                  # Express REST API
│   │   ├── .replit-artifact/
│   │   │   └── artifact.toml        # API service config (PORT=8080)
│   │   ├── src/
│   │   │   ├── index.ts             # Server entry
│   │   │   ├── app.ts               # Express app setup
│   │   │   ├── lib/
│   │   │   │   └── logger.ts        # Pino logger
│   │   │   ├── middlewares/
│   │   │   └── routes/
│   │   │       ├── index.ts         # Route aggregator
│   │   │       ├── health.ts
│   │   │       ├── farmers.ts
│   │   │       ├── harvests.ts
│   │   │       ├── spoilage.ts
│   │   │       ├── cold-stores.ts
│   │   │       ├── cold-stores-logic.ts
│   │   │       ├── bookings.ts
│   │   │       ├── transport.ts
│   │   │       ├── dashboard.ts
│   │   │       ├── analytics.ts
│   │   │       ├── weather.ts
│   │   │       ├── notifications.ts
│   │   │       ├── whatsapp.ts
│   │   │       └── n8n.ts
│   │   ├── build.mjs                # esbuild bundler
│   │   ├── dist/                    # Compiled output
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   └── mockup-sandbox/              # UI mockup preview sandbox
│       ├── .replit-artifact/
│       │   └── artifact.toml        # Mockup service (PORT=8081)
│       ├── src/
│       │   ├── .generated/
│       │   │   └── mockup-components.ts
│       │   ├── components/
│       │   │   ├── mockups/
│       │   │   └── ui/
│       │   ├── hooks/
│       │   └── lib/
│       ├── package.json
│       └── vite.config.ts
│
├── lib/                             # Shared libraries
│   │
│   ├── db/                          # Database layer
│   │   ├── src/
│   │   │   ├── index.ts             # Pool + Drizzle client
│   │   │   └── schema/
│   │   │       ├── index.ts
│   │   │       ├── farmers.ts
│   │   │       ├── harvests.ts
│   │   │       ├── cold-stores.ts
│   │   │       ├── bookings.ts
│   │   │       ├── transport.ts
│   │   │       ├── notifications.ts
│   │   │       └── weather.ts
│   │   ├── drizzle.config.ts
│   │   └── package.json
│   │
│   ├── api-spec/                    # OpenAPI source of truth
│   │   ├── openapi.yaml
│   │   ├── orval.config.ts
│   │   └── package.json
│   │
│   ├── api-client-react/            # Generated React Query hooks
│   │   ├── src/
│   │   │   ├── index.ts
│   │   │   └── generated/
│   │   └── package.json
│   │
│   └── api-zod/                     # Generated Zod schemas
│       ├── src/
│       │   ├── index.ts
│       │   └── generated/
│       │       └── types/
│       └── package.json
│
└── scripts/                         # Workspace utility scripts
    ├── src/
    │   └── hello.ts
    └── package.json
```

---

## Workspace Packages

| Package | Path | Purpose |
|---------|------|---------|
| `@workspace/kisan-cold-chain` | `artifacts/kisan-cold-chain` | Main React frontend |
| `@workspace/api-server` | `artifacts/api-server` | Express REST API |
| `@workspace/mockup-sandbox` | `artifacts/mockup-sandbox` | UI mockup sandbox |
| `@workspace/db` | `lib/db` | PostgreSQL + Drizzle schema |
| `@workspace/api-spec` | `lib/api-spec` | OpenAPI spec + Orval codegen |
| `@workspace/api-client-react` | `lib/api-client-react` | Generated API hooks |
| `@workspace/api-zod` | `lib/api-zod` | Generated Zod types |
| `@workspace/scripts` | `scripts` | Dev utility scripts |

---

## Getting Started

### Prerequisites

- **Node.js 24** (recommended; project targets Replit's Node 24 stack)
- **pnpm 9+**
- **PostgreSQL 16** (for API server)

### Install

```bash
pnpm install
```

### Environment Variables

| Variable | Required by | Description |
|----------|-------------|-------------|
| `DATABASE_URL` | API server, `lib/db` | PostgreSQL connection string |
| `PORT` | Web app, API server | Service port (`24832` web, `8080` API) |
| `BASE_PATH` | Web app | URL base path (e.g. `/`) |
| `NODE_ENV` | API server | `development` or `production` |
| `LOG_LEVEL` | API server | Pino log level (default: `info`) |

### Run Locally

**Frontend (web app):**

```bash
# Windows PowerShell
$env:PORT="24832"; $env:BASE_PATH="/"
pnpm --filter @workspace/kisan-cold-chain run dev
```

Open [http://localhost:24832](http://localhost:24832)

**API server:**

```bash
$env:PORT="8080"; $env:DATABASE_URL="postgresql://user:pass@localhost:5432/kisan"
pnpm --filter @workspace/api-server run dev
```

**Mockup sandbox:**

```bash
$env:PORT="8081"; $env:BASE_PATH="/__mockup"
pnpm --filter @workspace/mockup-sandbox run dev
```

### Other Commands

```bash
pnpm run typecheck                              # Full workspace typecheck
pnpm run build                                  # Typecheck + build all packages
pnpm --filter @workspace/api-spec run codegen   # Regenerate API hooks + Zod schemas
pnpm --filter @workspace/db run push            # Push DB schema (dev only)
```

---

## Application Routes

| Route | Access | Description |
|-------|--------|-------------|
| `/` | Public | Landing page |
| `/login` | Public | Demo login |
| `/signup` | Public | Registration |
| `/farmer` | Farmer, Admin | Farmer dashboard |
| `/farmer/harvests` | Farmer, Admin | Harvest list |
| `/farmer/harvest/new` | Farmer, Admin | Register new harvest |
| `/farmer/bookings` | Farmer, Admin | Booking history |
| `/spoilage` | Farmer, Admin | AI spoilage prediction |
| `/cold-stores` | Farmer, Operator, Admin | Cold store discovery |
| `/transport` | Farmer, Admin | Transport pooling |
| `/transport/dashboard` | Transport, Admin | Transport provider dashboard |
| `/operator` | Operator, Admin | Cold store operator dashboard |
| `/admin` | Admin | Platform admin dashboard |
| `/admin/analytics` | Admin | Analytics & insights |
| `/whatsapp` | Farmer, Admin | WhatsApp bot interface |
| `/n8n` | Admin | n8n automation blueprints |
| `/notifications` | All roles | Notifications center |

---

## API Endpoints

Base path: `/api`

| Tag | Routes |
|-----|--------|
| `health` | `GET /healthz` |
| `farmers` | Farmer CRUD |
| `harvests` | Harvest registration |
| `spoilage` | AI spoilage prediction |
| `cold-stores` | Store discovery & management |
| `bookings` | Storage bookings |
| `transport` | Transport pooling & jobs |
| `dashboard` | Role-based summaries |
| `analytics` | Platform analytics |
| `weather` | Weather & risk data |
| `notifications` | Notification management |
| `whatsapp` | WhatsApp webhook |
| `n8n` | Workflow blueprints |

Full contract: `lib/api-spec/openapi.yaml`

---

## User Roles

| Role | Dashboard | Key capabilities |
|------|-----------|------------------|
| **Farmer** | `/farmer` | Register harvests, AI spoilage alerts, book cold stores, pool transport |
| **Operator** | `/operator` | Manage cold store capacity, approve bookings, revenue analytics |
| **Transport** | `/transport/dashboard` | Accept pooled jobs, track deliveries |
| **Admin** | `/admin` | Platform analytics, user management, n8n automation |

---

## License

MIT
