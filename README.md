# my-first-project
import requests
#Constants for the weather API
API_URL = "https://api.openweathermap.org/data/2.5/weather" # Corrected API endpoint
API_KEY = "b2d562b9b2df6a3e1d740eb4da3043a7"

def fetch_weather_data(location):
    #Build the request URL
    request_url = f"{API_URL}?q={location}&APPID={API_KEY}&units=metric"

    #Send the request to the API
    response = requests.get(request_url)

    # Check for network errors
    if response.status_code != 200:
        print(f"API request failed with status code: {response.status_code}")
        return None, response.text  # Return the raw response for debugging

    #Parse the JSON response
    try:
        weather_data = response.json()
    except ValueError as e:
        print(f"Error decoding JSON response: {e}")
        return None, response.text  # Return the raw response for debugging

    #Extract relevant weather data
    temperature = weather_data.get("main", {}).get("temp")
    weather_conditions = weather_data.get("weather", [{}])[0].get("description")
    humidity = weather_data.get("main", {}).get("humidity")
    wind_speed = weather_data.get("wind", {}).get("speed")

    return {
        "temperature": temperature,
        "weather_conditions": weather_conditions,
        "humidity": humidity,
        "wind_speed": wind_speed
    }, None

def display_weather_data(weather_data):
    print(f"Temperature: {weather_data['temperature']}°C")
    print(f"Weather Conditions: {weather_data['weather_conditions']}")
    print(f"Humidity: {weather_data['humidity']}%")
    print(f"Wind Speed: {weather_data['wind_speed']} m/s")
def main():
    # Get the location input from the user
    location = input("Enter the city name or coordinates (lat,lon): ")

    # Fetch the weather data for the input location
    weather_data, error = fetch_weather_data(location)

    if error:
        print(f"Error: {error}")
    else:
        # Display the fetched weather data
        display_weather_data(weather_data)

if __name__ == "__main__":
    main()
