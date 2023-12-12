<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Servo Control with Weather</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            text-align: center;
            margin: 20px;
            background-color: #f5f5f5;
            color: #333;
        }
        h1, h2 {
            color: #008080; /* Teal color for headers */
        }
        label {
            display: block;
            margin: 10px 0;
        }
        input[type="number"] {
            width: 50px;
            padding: 5px;
            margin-bottom: 10px;
        }
        input[type="range"] {
            width: 80%;
            margin: 0 auto;
        }
        button {
            background-color: #4CAF50;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        #weather-container {
            margin-top: 20px;
            border: 1px solid #ccc;
            padding: 10px;
            background-color: #fff;
        }
        #weather-description, #temperature {
            font-size: 18px; /* Increased font size for temperature */
            color: #333;
        }
    </style>
</head>
<body>
    <h1>Servo Control with Weather</h1>
    <label for="positionInput">Enter Position (0-180, increments of 10): </label>
    <input type="number" id="positionInput" min="0" max="180" step="10">
    
    <label for="positionSlider">Or use the slider:</label>
    <input type="range" id="positionSlider" min="0" max="180" step="10">
    
    <button onclick="sendPosition()">Set Position</button>

    <div id="weather-container">
        <h2>Weather Information</h2>
        <p id="weather-description"></p>
        <p id="temperature"></p>
    </div>

    <script>
        document.addEventListener("DOMContentLoaded", function() {
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
                        } else {
                            console.error('Failed to set servo position');
                        }
                    })
                    .catch(error => console.error('Error:', error));
                } else {
                    console.error('Invalid position. Please enter a value between 0 and 180, in increments of 10.');
                }
            }

            // Update the input field value when the slider changes
            document.getElementById("positionSlider").addEventListener("input", function() {
                document.getElementById("positionInput").value = this.value;
            });

            // Fetch weather information from OpenWeatherMap API
            function fetchWeather() {
                const apiKey = 'YOUR_API_KEY'; // Replace with your OpenWeatherMap API key
                const city = 'YOUR_CITY'; // Replace with your desired city
                const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}`;

                fetch(apiUrl)
                    .then(response => response.json())
                    .then(data => {
                        // Update weather information on the page
                        document.getElementById("weather-description").textContent = `Weather: ${data.weather[0].description}`;
                        const temperature = (data.main.temp - 273.15).toFixed(2); // Convert temperature to Celsius
                        document.getElementById("temperature").textContent = `Temperature: ${temperature} °C`;
                    })
                    .catch(error => console.error('Error fetching weather:', error));
            }

            // Initial fetch when the page loads
            fetchWeather();

            // Update weather every 30 minutes (adjust as needed)
            setInterval(fetchWeather, 1800000); // 30 minutes in milliseconds
        });
    </script>
</body>
</html>
