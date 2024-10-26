The Weather Monitoring System is a Spring Boot-based application designed to fetch and manage weather data for specified cities. It provides daily summaries, alert notifications based on temperature and weather conditions, and supports flexible querying and rolling up of weather data.

Features
Fetches real-time weather data from the OpenWeatherMap API for multiple cities at regular intervals.
Stores daily weather data, including temperature, humidity, wind speed, and weather conditions.
Supports configurable units (Celsius, Fahrenheit, Kelvin) for temperature.
Generates daily summaries for each city with minimum, maximum, and average temperatures, and the dominant weather condition.
Sends alerts when specified weather thresholds are exceeded.
Technologies Used
Java with Spring Boot
Spring Data JPA for database operations
MySQL (or any relational database)
Postman for API testing
Setup Instructions
Clone the Repository

bash
Copy code
git clone <repository-url>
cd WeatherMonitoringSystem
Configure the Application Properties

Open the src/main/resources/application.properties file.
Set up your database and API key configurations:
properties
Copy code
spring.datasource.url=jdbc:mysql://localhost:3306/weather_db
spring.datasource.username=root
spring.datasource.password=yourpassword
spring.jpa.hibernate.ddl-auto=update

# OpenWeatherMap API Key
weather.api.key=YOUR_API_KEY
Run the Application

bash
Copy code
./mvnw spring-boot:run
Verify the API Documentation (Optional)


Testing with Postman
1. Fetch Weather Data for a Specific City
Endpoint: GET api/weather?city={cityName}&unit={unit}
Parameters:
cityName: (required) Name of the city.
unit: (optional) "metric" for Celsius, "imperial" for Fahrenheit, or "kelvin" for Kelvin (default is metric).
Example Request:
bash
Copy code
GET http://localhost:8080/api/weather?city=Delhi&unit=metric
Expected Output:
JSON object containing current weather data for the specified city.
2. Get Daily Weather Summary for a City
Endpoint: GET api/daily-summary
Parameters:
city: (required) Name of the city.
date: (required) Date in yyyy-MM-dd format for the daily summary.
Example Request:
bash
Copy code
GET http://localhost:8080/api/daily-summary?city=Delhi&date=2023-10-26
Expected Output:
JSON object containing the daily summary with fields like average temperature, maximum temperature, minimum temperature, and dominant weather condition.
3. Simulate Weather Data Roll-up (Scheduled Task)
This is an automatic feature that runs every 5 minutes to fetch and save weather data for configured cities.
Manual Trigger (Optional): To test manually, you can trigger this functionality by invoking:
java
Copy code
weatherService.fetchWeatherData(); // Only for manual testing in a development environment
4. Generate Alerts Based on Thresholds
Endpoint: POST /api/alerts
Body Parameters:
json
Copy code
{
  "city": "Delhi",
  "temperatureThreshold": 40,
  "condition": "Rain"
}
Example Request:
bash
Copy code
POST http://localhost:8080/api/alerts
Body:
{
  "city": "Delhi",
  "temperatureThreshold": 40,
  "condition": "Rain"
}
Expected Output:
JSON response indicating whether the specified city exceeded the threshold conditions and the generated alert, if any.
Example JSON Responses
Fetch Weather Data Response
json
Copy code
{
  "city": "Delhi",
  "temperature": 25.6,
  "feels_like": 27.0,
  "humidity": 60,
  "condition": "Clear",
  "wind_speed": 5.1
}
Daily Summary Response
json
Copy code
{
  "city": "Delhi",
  "date": "2023-10-26",
  "avgTemperature": 26.3,
  "maxTemperature": 29.1,
  "minTemperature": 24.7,
  "dominantCondition": "Clear"
}
Alert Response
json
Copy code
{
  "alert": "High Temperature Alert",
  "message": "Delhi is experiencing very high temperatures!"
}
Scheduled Weather Data Roll-Up Process
Every 5 minutes, the application will:

Fetch the latest weather data from OpenWeatherMap for each city.
Update or create the daily weather summary for each city.
Calculate average, minimum, and maximum temperatures, as well as the dominant weather condition.
Check thresholds and generate alerts if conditions are met.
Future Enhancements
Integration with a frontend application for real-time data display.
Addition of SMS or email notifications for alerts.
Enhanced reporting capabilities for historical weather data analytics.
