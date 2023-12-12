<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Servo Control with Weather</title>
    <style>
        /* Your existing styles remain unchanged */

        #weather {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <h1>Servo Control with Weather</h1>
    <label for="positionInput">Enter Servo Position (0-180, increments of 10): </label>
    <input type="number" id="positionInput" min="0" max="180" step="10">
    
    <label for="positionSlider">Or use the slider:</label>
    <input type="range" id="positionSlider" min="0" max="180" step="10">
    
    <button onclick="sendPosition()">Set Position</button>

    <div id="weather">
        <h2>Weather Information</h2>
        <p id="weatherData">Loading weather data...</p>
    </div>

    <script>
        function sendPosition() {
            var position = document.getElementById("positionInput").value;
            if (position >= 0 && position <= 180 && position % 10 === 0) {
                // Send a POST request to the agent with the specified position
                fetch('https://agent.electricimp.com/j9_euFWlOzzN', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/x-www-form-urlencoded'
                    },
                    body: 'position=' + position
                })
                .then(response => {
                    if (response.ok) {
                        console.log('Servo position set successfully');
                        fetchWeather(); // Fetch weather data after setting the servo position
                    } else {
                        console.error('Failed to set servo position');
                    }
                })
                .catch(error => console.error('Error:', error));
            } else {
                console.error('Invalid position. Please enter a value between 0 and 180, in increments of 10.');
            }
        }

        function fetchWeather() {
            var apiKey = 'afa92ea67fe55fdfa48347e9f635fda6'; // Replace with your OpenWeatherMap API key
            var weatherLocation = 'Barrie'; // Replace with the desired weather location

            fetch('https://api.openweathermap.org/data/2.5/weather?q=' + weatherLocation + '&appid=' + apiKey)
                .then(response => response.json())
                .then(data => {
                    var weatherDescription = data.weather[0].description;
                    var temperature = (data.main.temp - 273.15).toFixed(2);
                    var weatherDataElement = document.getElementById("weatherData");
                    weatherDataElement.innerHTML = 'Weather: ' + weatherDescription + '<br>Temperature: ' + temperature + '°C';
                })
                .catch(error => {
                    console.error('Error fetching weather data:', error);
                    var weatherDataElement = document.getElementById("weatherData");
                    weatherDataElement.innerHTML = 'Failed to fetch weather data. Check the console for details.';
                });
        }

        document.getElementById("positionSlider").addEventListener("input", function() {
            document.getElementById("positionInput").value = this.value;
            fetchWeather();
        });
    </script>
</body>
</html>
