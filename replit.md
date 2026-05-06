# Weather Forecast — Rails App

A Ruby on Rails weather forecast app that accepts an address, geocodes it, fetches real weather data, and caches results by zip code for 30 minutes.

## Run & Operate

- `cd weather-forecast && PORT=3000 bundle exec rails server -b 0.0.0.0 -p 3000 -e development` — run the app (workflow: "Start application")
- `cd weather-forecast && bundle install` — install gems
- `cd weather-forecast && bundle exec bootsnap precompile --gemfile app/ lib/` — pre-warm bootsnap cache (speeds up boot)
- No API keys needed — uses free Nominatim (OSM) + Open-Meteo APIs

## Stack

- Ruby 3.2.2 + Rails 8.1.3
- Puma 8.0.1 (web server, binds to 0.0.0.0:PORT)
- Rails memory cache store (30-min TTL, keyed by zip code)
- Geocoding: Nominatim (OpenStreetMap) — free, no key
- Weather: Open-Meteo — free, no key
- No database (--skip-active-record)

## Where things live

```
weather-forecast/
├── app/
│   ├── controllers/forecasts_controller.rb   — address → geocode → cache → weather
│   ├── services/geocoding_service.rb         — Nominatim API (lat/lon + zip)
│   ├── services/weather_service.rb           — Open-Meteo API (current + 7-day)
│   └── views/forecasts/
│       ├── new.html.erb                       — address input form
│       └── show.html.erb                      — forecast display + cache badge
├── config/
│   ├── routes.rb                              — root → forecasts#new, /forecast/show
│   └── puma.rb                                — binds to 0.0.0.0:PORT
```

## Architecture decisions

- **Contract-first caching**: cache key is `weather_forecast_v1_<zip>` — if zip unavailable, falls back to `lat,lon`
- **Rails.cache (memory_store)**: 30-minute TTL, cache indicator shown on every response
- **No DB**: stateless app — only external APIs + Rails cache
- **Free APIs only**: Nominatim (OSM) for geocoding, Open-Meteo for weather — no API keys
- **Service objects**: GeocodingService and WeatherService are plain Ruby classes in app/services/

## Product

- User enters any address / city / zip code
- App geocodes via Nominatim → gets lat/lng + zip code
- Checks Rails memory cache by zip (30-min TTL)
- Shows current temp (°F), feels-like, humidity, wind, high/low, 7-day forecast
- Green "Live data" badge on fresh fetch; amber "Served from cache" badge on cache hit

## Gotchas

- Bootsnap cache pre-warming (`bundle exec bootsnap precompile`) is required before first workflow start or boot is slow
- Rails binds to `0.0.0.0` via `-b 0.0.0.0` CLI flag (puma.rb `bind` line is a fallback)
- Workflow outputType: webview, waitForPort: 3000

## Pointers

- See the `pnpm-workspace` skill for workspace structure details (existing Node.js artifacts)
- Rails app lives in `weather-forecast/` at the workspace root (separate from pnpm packages)
