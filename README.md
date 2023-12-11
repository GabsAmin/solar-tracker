
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
        }
        h1 {
            color: #333;
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
        #weatherInfo {
            margin-top: 20px;
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

    <div id="weatherInfo"></div>

    <script>
        // Replace 'YOUR_API_KEY' with your actual OpenWeatherMap API key
        const apiKey = 'afa92ea67fe55fdfa48347e9f635fda6';
        const city = 'Barrie'; // Update with your city

        function fetchWeather() {
            const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;

            fetch(apiUrl)
                .then(response => {
                    if (!response.ok) {
                        throw new Error(`HTTP error! Status: ${response.status}`);
                    }
                    return response.json();
                })
                .then(data => {
                    if (!data.main || !data.main.temp || !data.weather || data.weather.length === 0) {
                        throw new Error('Invalid weather data format');
                    }

                    const temperature = data.main.temp;
                    const weatherDescription = data.weather[0].description;

                    const weatherInfo = `Current Temperature: ${temperature}°C<br>Weather: ${weatherDescription}`;

                    document.getElementById("weatherInfo").innerHTML = weatherInfo;
                })
                .catch(error => {
                    console.error('Error fetching weather:', error.message);
                    document.getElementById("weatherInfo").innerHTML = 'Failed to fetch weather';
            
