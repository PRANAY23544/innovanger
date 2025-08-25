# Smart Recycling Hub (Haldwani)

A production-ready Next.js (App Router, TypeScript) + Tailwind CSS platform for smart waste management in Haldwani. Track live smart bins on an interactive map (Leaflet + OpenStreetMap), find the nearest available bin with geolocation, read and submit reviews, and manage bins via a simple dashboard.

## Features
- Live Map (Leaflet + OSM)
  - Color-coded bins with black-outlined markers for clarity
  - Find Nearest Bin (uses browser geolocation)
  - User location marker and straight-line route polyline to the chosen bin
  - “Open in Maps” deep link to Google Maps walking directions
  - Legend overlay and last-updated badge (auto-refresh every 30s)
  - Fullscreen mode (ESC to exit) and Refresh button with loading state
- Dashboard
  - Sort/filter bins; highlights high-priority (Full or ≥80%)
  - Click a row to open the map focused on that bin with directions
- Reviews
  - Submit rating and message; persisted locally (localStorage)
  - Live-updating reviews list and average rating
- Contact API
  - Simple route handler that validates payload and returns success
- Modern UI & A11y
  - Tailwind CSS v4, responsive, dark-mode friendly
  - Accessible labels, keyboard-friendly interactions

## Tech Stack
- Next.js 15 (App Router), React 19, TypeScript 5
- Tailwind CSS v4, PostCSS
- Leaflet 1.9, React-Leaflet 5, OpenStreetMap tiles
- Next.js Route Handlers for APIs

See `techstact.md` for the full detailed stack and architecture notes.

## Project Structure
```
src/
  app/
    page.tsx             # Home (hero + map)
    dashboard/page.tsx   # Dashboard
    reviews/page.tsx     # Reviews
    about/page.tsx       # About
    contact/page.tsx     # Contact
    api/
      bins/route.ts      # GET /api/bins
      contact/route.ts   # POST /api/contact
  components/
    BinMap.tsx           # React-Leaflet map (geolocation, route, fullscreen)
    MapSection.tsx       # Client-only wrapper for map (SSR-safe)
    DashboardTable.tsx   # Dashboard table
    Navbar.tsx, Footer.tsx
    ReviewForm.tsx, ReviewCard.tsx, ClientReviews.tsx, RatingStars.tsx
  lib/
    bins.ts              # Bin data + helpers (Haversine, nearest, jitter)
    reviews.ts           # LocalStorage persistence for reviews
```

## Getting Started (Windows PowerShell)
1. Install dependencies
```powershell
npm install
```
2. Run the dev server (Turbopack)
```powershell
npm run dev
```
- The app will start on the first free port (e.g., http://localhost:3000 or http://localhost:3001)
3. Build for production
```powershell
npm run build
```
4. Start production server
```powershell
npm start
```

## Environment
- No map API keys required (uses OpenStreetMap tiles).
- Reviews use browser localStorage.

## Usage Tips
- Dashboard → Map directions: click any bin row; it opens `/?bin=BIN_ID&directions=1`.
- On the map, use “Find Nearest Bin” to locate the closest available bin; allow location access.
- Use the “Fullscreen” button for an immersive map view; press ESC to exit.

## API
- `GET /api/bins` — Returns simulated IoT bin data with jitter and timestamps.
- `POST /api/contact` — Accepts `{ name, email, message }`; returns `{ success: true }` on valid payload.

## Accessibility & Performance
- ARIA labels for buttons; keyboard/ESC support for fullscreen.
- Lightweight map controls; SSR-safe by client-only dynamic wrapper.

## Troubleshooting
- If the map doesn’t appear in production, ensure `MapSection.tsx` is used (client-only dynamic import) and Leaflet CSS is imported in `src/app/globals.css`.
- If geolocation is denied, the app will open Google Maps with the destination only.

## License
MIT (or your preferred license)
