# Weather Forecast Using Rails

A Ruby on Rails application that allows users to search weather forecasts using an address or zip code. The application retrieves live weather data, displays forecast details, and caches responses for improved performance.

## Live Demo

Access the deployed application here:

https://weather-forecast-using-replit.onrender.com/

---

## Features

- Search weather forecast using address or zip code
- Displays:
  - Current temperature
  - Weather condition
  - High / Low temperatures
  - Extended forecast (if available)
- Zip-code based caching for 30 minutes
- Cache indicator to show whether data is served from cache
- Clean and simple UI
- Built with Ruby on Rails

---

## Requirements

- Ruby 3.x
- Rails 7.x
- Redis (optional for production caching)
- PostgreSQL or SQLite

---

## Setup Instructions

### Clone the repository

```bash
git clone https://github.com/veeru4soft/weather-forecast-using-replit.git
cd weather-forecast-using-replit
```

### Install dependencies

```bash
bundle install
```

### Setup database

```bash
rails db:create
rails db:migrate
```

### Start the Rails server

```bash
rails server
```

Application will be available at:

```txt
http://localhost:3000
```

---

## Environment Variables

Create a `.env` file or configure environment variables.

Example:

```env
WEATHER_API_KEY=your_api_key
```

If deployed to production:

```env
RAILS_MASTER_KEY=your_master_key
SECRET_KEY_BASE=your_secret_key_base
```

---

## Caching Strategy

The application caches weather responses by zip code for 30 minutes.

Example cache key:

```ruby
weather_forecast_90210
```

Benefits:
- Reduces external API calls
- Improves response time
- Enhances user experience

The UI also displays whether the result was fetched from:
- Live API
- Cache

---

## Tech Stack

- Ruby on Rails
- ERB
- PostgreSQL / SQLite
- External Weather API
- Rails Cache Store

---

## Project Structure

```txt
app/
 ├── controllers/
 ├── services/
 ├── views/
 ├── models/

config/
db/
```

---

## Assumptions

- Forecast data is fetched from a public weather API
- Address input is converted into latitude/longitude or zip code
- Cache expiration is fixed to 30 minutes
- Functionality is prioritized over UI styling

---

## Future Improvements

- Add 5-day forecast
- Add weather icons
- Add geolocation support
- Add unit conversion (°C / °F)
- Add test coverage using RSpec
- Docker support
- Background jobs for forecast refresh

---

## Deployment

The application can be deployed using:

- Render
- Heroku
- Railway
- Fly.io

Production URL:

https://weather-forecast-using-replit.onrender.com/

---

## Screenshots

_Add application screenshots here_

---

## Author

Developed by Veerababu

GitHub Repository:
https://github.com/veeru4soft/weather-forecast-using-replit

---

## License

This project is open source and available under the MIT License.
