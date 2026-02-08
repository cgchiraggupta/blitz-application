# How This App Works

## Stack

- **Frontend:** React 18 + TypeScript, Vite, React Router
- **UI:** Tailwind CSS, Radix/shadcn components, Framer Motion
- **Backend / DB / Auth:** Supabase (PostgreSQL, Auth, Storage, RLS)
- **State:** React Query (TanStack Query), React Context (auth, theme)

## Credentials

The app needs **Supabase** to run. No credentials are committed (they go in `.env`).

1. Copy `.env.example` to `.env`
2. In [Supabase Dashboard](https://supabase.com/dashboard) → your project → **Settings → API** copy:
   - **Project URL** → `VITE_SUPABASE_URL`
   - **anon public** key → `VITE_SUPABASE_ANON_KEY`
3. Save `.env` in the project root (same folder as `package.json`)

## Run Locally

```bash
npm install
npm run dev
```

Then open the URL Vite prints (e.g. http://localhost:5173).

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│  React App (Vite)                                            │
│  - Pages: Feed, Products, Cart, Checkout, Orders, Profile…   │
│  - Auth (AuthContext) + Theme (ThemeContext)                 │
│  - React Query for server state                              │
└───────────────────────────┬─────────────────────────────────┘
                            │
                            │  @supabase/supabase-js
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  Supabase                                                    │
│  - Auth (email/password, profiles)                            │
│  - PostgreSQL (products, orders, cart, wishlist, groups…)    │
│  - Storage (product images)                                   │
│  - RLS (row-level security)                                   │
└─────────────────────────────────────────────────────────────┘
```

## Main Features

- **Auth:** Sign up / sign in via Supabase Auth; profile (name, role, avatar) in `profiles` table
- **Roles:** `user` (shopper), `vendor` (seller), `admin` (dashboard)
- **Products:** Vendors add products; shoppers browse, search, filter by category
- **Cart / Wishlist:** Per-user; stored in Supabase
- **Checkout / Orders:** Orders and order items in DB; optional group orders
- **Groups:** Private group orders (create group, invite, bulk discount)
- **Social:** Feed, user profiles, posts, comments (if enabled)
- **Dashboard:** User / Vendor / Admin dashboards (e.g. `/dashboard`)

## Important Folders

| Folder | Purpose |
|--------|--------|
| `src/pages/` | Route-level pages (Feed, Products, Cart, Checkout, etc.) |
| `src/components/` | Reusable UI (Header, Footer, ProductCard, modals, dashboards) |
| `src/contexts/` | AuthContext, ThemeContext |
| `src/integrations/supabase/` | Supabase client and TypeScript types |
| `src/hooks/` | Custom hooks (e.g. useDebounce, useProductRating) |
| `supabase/migrations/` | SQL migrations for tables, RLS, functions |

## Build & Deploy

- **Build:** `npm run build` → output in `dist/`
- **Preview build:** `npm run preview`
- Deploy `dist/` to any static host (Vercel, Netlify, etc.). Point Supabase to the same project (same `.env` values) so auth and API work in production.
