# Tech Stack

This document lists the technologies, libraries, tools, and notable patterns used in this project.

## Frameworks & Languages
- Next.js 15 (App Router)
- React 19
- TypeScript 5

## Styling & UI
- Tailwind CSS v4
- PostCSS (via `@tailwindcss/postcss`)
- Responsive, accessible components with Tailwind utilities

## Mapping & Geolocation
- Leaflet 1.9 (interactive maps)
- React-Leaflet 5 (Leaflet bindings for React)
- OpenStreetMap tiles (`https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png`)
- Leaflet CSS imported globally in `src/app/globals.css`
- Browser Geolocation API (find user location)
- Fullscreen API (enter/exit fullscreen map)
- Features implemented in `src/components/BinMap.tsx`:
  - Color-coded bins with black-outlined markers for clarity
  - User location marker
  - Polyline route from user to selected/nearest bin (straight-line demo)
  - Auto “Find nearest bin”, ETA calculation, and fit-to-bounds
  - Legend overlay, last-updated badge, refresh button with loading state
  - Real fullscreen toggle (ESC supported) and body scroll lock

## Data & Utilities
- `src/lib/bins.ts`
  - Dummy bin data (Haldwani)
  - `distanceMeters` (Haversine)
  - `findNearestAvailable`
  - Jitter/timestamp helpers
- `src/lib/reviews.ts`
  - Local storage persistence (`window.localStorage`)
  - Average rating computation

## API Routes (Next.js Route Handlers)
- `GET /api/bins` — returns jittered/timestamped bin data (simulated IoT feed)
- `POST /api/contact` — validates `{ name, email, message }`, returns success

## Pages (App Router)
- `/` Home — hero + live map (Map wrapped client-only via `MapSection`)
- `/dashboard` — sortable/filterable bin table with deep links to map + directions
- `/reviews` — review form and list (client-rendered)
- `/about`, `/contact`

## Components
- `Navbar`, `Footer`
- `RatingStars`, `ReviewForm`, `ReviewCard`, `ClientReviews`
- `DashboardTable`
- `BinMap` (React-Leaflet map with geolocation, directions, fullscreen, refresh)
- `MapSection` (client-only dynamic wrapper for `BinMap` to avoid SSR issues)

## Accessibility & UX
- ARIA labels on interactive controls
- Mobile nav drawer with `aria-expanded`/`aria-controls` and body scroll lock
- Keyboard/ESC support for exiting fullscreen
- Clear focus styles and high-contrast UI choices

## External Links & Services
- “Open in Maps” uses Google Maps Directions URL for walking routes (external)
- Map tiles from OpenStreetMap

## Tooling & Build
- Next.js Turbopack (`dev`/`build` scripts)
- ESLint 9 with `eslint-config-next`
- TypeScript config (`tsconfig.json`)
- Tailwind CSS config (`postcss.config.mjs`, classes in `globals.css`)

## Types
- `@types/leaflet`, `@types/node`, `@types/react`, `@types/react-dom`

## Scripts (from `package.json`)
- `dev`: `next dev --turbopack`
- `build`: `next build --turbopack`
- `start`: `next start`
- `lint`: `eslint`

## Notable Patterns
- Client-only dynamic import for map (`MapSection`) to keep SSR safe
- Query-driven deep links: `/?bin=BIN_ID&directions=1` (dashboard → map)
- Periodic refresh: map (30s), dashboard (10s)
- Local state + localStorage for reviews, with custom `reviews:changed` event

## Notes
- Previous Google Maps API key is not required with Leaflet/OSM. Any legacy Google Maps shims/env can be removed if present.
