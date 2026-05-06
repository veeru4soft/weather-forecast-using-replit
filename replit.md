# Weather Forecast — Rails App

A Ruby on Rails weather forecast app that accepts an address, geocodes it, fetches real weather data, and caches results by zip code for 30 minutes.

## Run & Operate

- Workflow: **"Start application"** — runs automatically on boot
- Command: `SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt PORT=3000 bundle exec rails server -b 0.0.0.0 -p 3000 -e development`
- `bundle install` — install gems after changes
- No API keys needed — uses free Nominatim (OSM) + Open-Meteo APIs

## Stack

- Ruby 3.2.2 + Rails 8.1.3
- Puma 8.0.1 (web server, binds to 0.0.0.0:3000)
- Rails memory cache store (30-min TTL, keyed by zip code)
- Geocoding: Nominatim (OpenStreetMap) — free, no key
- Weather: Open-Meteo — free, no key
- No database (--skip-active-record)

## Where things live

```
/                                               ← Rails root (repo root)
├── app/
│   ├── controllers/forecasts_controller.rb    — address → geocode → cache → weather
│   ├── services/geocoding_service.rb          — Nominatim API (lat/lon + zip)
│   ├── services/weather_service.rb            — Open-Meteo API (current + 7-day)
│   └── views/forecasts/
│       ├── new.html.erb                        — address input form
│       └── show.html.erb                       — forecast display + cache badge
├── config/
│   ├── routes.rb                               — root → forecasts#new, /forecast/show
│   └── puma.rb                                 — binds to 0.0.0.0:PORT
├── Gemfile / Gemfile.lock
└── Dockerfile
```

## Architecture decisions

- **Contract-first caching**: cache key is `weather_forecast_v1_<zip>` — if zip unavailable, falls back to `lat,lon`
- **Rails.cache (memory_store)**: 30-minute TTL, cache indicator shown on every response
- **No DB**: stateless app — only external APIs + Rails cache
- **Free APIs only**: Nominatim (OSM) for geocoding, Open-Meteo for weather — no API keys
- **Service objects**: GeocodingService and WeatherService are plain Ruby classes in app/services/
- **SSL cert**: `SSL_CERT_FILE` env var points to `/etc/ssl/certs/ca-certificates.crt` for Nix compatibility

## Product

- User enters any address / city / zip code
- App geocodes via Nominatim → gets lat/lng + zip code
- Checks Rails memory cache by zip (30-min TTL)
- Shows current temp (°F), feels-like, humidity, wind, high/low, 7-day forecast
- Green "Live data" badge on fresh fetch; amber "Served from cache" badge on cache hit

## Gotchas

- `SSL_CERT_FILE` must be set for HTTPS calls to work in Nix/Replit environment
- Rails binds to `0.0.0.0` via `-b 0.0.0.0` CLI flag so Replit proxy can reach it
- Port 3000 maps to external port 80
- Rails project is at the **repo root** (not a subdirectory) — required for Render.com auto-detection

## User preferences

- Pure Ruby on Rails only — no Node.js, no JavaScript build tools
