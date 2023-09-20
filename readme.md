# PHP Weather Forecast Library

The PHP Weather Forecast Library is a PHP package that provides weather forecast information based on geolocation data. It uses external services to retrieve geolocation and weather forecast data.

## Installation

Get an API Key from Open Weather API. Visit https://openweathermap.org/api/one-call-3 for more information.

You can install this library via Composer. Run the following command:

```bash
composer require stuarttodd-dev/WeatherForecastAPI
```
## Static Usage

```

use StuartToddDev\WeatherForecast\Services\WeatherForecast;

// Initialize the geolocation and weather forecast services
$geolocationAPI = new GeolocationIPAPI('your_geolocation_api_key');
$forecastAPI = new OpenWeatherAPI('your_weather_forecast_api_key');

// Initialize the WeatherForecast class
$weatherForecast = WeatherForecast::create('your-ip-address', 'your-open-weather-api-key');

$weatherForecast->getIpAddress(); // returns ip
$weatherForecast->getGeolocation(); // returns ['lon' => 'your-lon', 'lat' => 'your-lat']
$weatherForecast->getForecast(); // Json string from open weather API (see https://openweathermap.org/api/one-call-3)
```

## Inject API Services Usage

```
use StuartToddDev\WeatherForecast\Services\WeatherForecast;
use StuartToddDev\WeatherForecast\Services\GeolocationIPAPI;
use StuartToddDev\WeatherForecast\Services\OpenWeatherAPI;

// Initialize the geolocation and weather forecast services
$geolocationAPI = new GeolocationIPAPI(new Client()); // first param is Guzzle client
$forecastAPI = new OpenWeatherAPI(new Client()'); // first param is Guzzle client

You can inject different API services (or adjust them) as long as they implement the relevant contracts (noted below).

// Initialize the WeatherForecast class
$weatherForecast = WeatherForecast::create('your-ip-address', 'your-open-weather-api-key', $geolocationAPI, $forecastAPI);
$weatherForecast->getIpAddress(); // returns ip
$weatherForecast->getGeolocation(); // returns ['lon' => 'your-lon', 'lat' => 'your-lat']
$weatherForecast->getForecast(); // Json string from open weather API (see https://openweathermap.org/api/one-call-3)
```

## ForecastContract
Used by OpenWeatherAPI
```
    public function setLon(string $lon): self;
    public function getLon(): string;
    public function setLat(string $lat): self;
    public function getLat(): string;
    public function setApiKey(string $apiKey): self;
    public function getForecast(): ?string;
```

## GeolocationContract
Used by GeolocationIPAPI
```
interface GeolocationContract
{
    public function setIPAddress(string $ipAddress): self;
    public function getIPAddress(): ?string;
    public function getGeoLocation(): ?string;
}
```

