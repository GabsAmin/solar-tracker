<!GeorgianCollege Tech Pro>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Solar Panel Control </title>
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
        #weather {
            margin-top: 20px;
        }
        #sliderContainer {
            margin-top: 10px;
        }
        #positionSlider {
            width: 80%;
            margin: 0 auto;
        }
        #feedbackMessage {
            margin-top: 10px;
            color: #333;
        }
    </style>
</head>
<body>
    <h1>Servo Control with Weather</h1>
    <label for="positionInput">Enter Servo Position (0-180, increments of 10): </label>
    <input type="number" id="positionInput" min="0" max="180" step="10" placeholder="Enter position">
    
    <div id="sliderContainer">
        <label for="positionSlider">Or use the slider:</label>
        <input type="range" id="positionSlider" min="0" max="180" step="10">
    </div>
    
    <button onclick="sendPosition()">Set Position</button>
    <button onclick="resetForm()">Reset</button>

    <div id="weather">
        <h2>Weather Information</h2>
        <p id="weatherData">Loading weather data...</p>
    </div>

    <p id="feedbackMessage"></p>

    <script>
        function sendPosition() {
            var position = document.getElementById("positionInput").value;
            var feedbackMessageElement = document.getElementById("feedbackMessage");

            if (position >= 0 && position <= 180 && position % 10 === 0) {
                // Send a POST request to the agent with the specified position
                feedbackMessageElement.innerText = 'Setting servo position...';
                
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
                        feedbackMessageElement.innerText = 'Servo position set successfully';
                        fetchWeather(); // Fetch weather data after setting the servo position
                    } else {
                        console.error('Failed to set servo position');
                        feedbackMessageElement.innerText = 'Failed to set servo position';
                    }
                })
                .catch(error => {
                    console.error('Error:', error);
                    feedbackMessageElement.innerText = 'Error. Check the console for details.';
                });
            } else {
                console.error('Invalid position. Please enter a value between 0 and 180, in increments of 10.');
                feedbackMessageElement.innerText = 'Invalid position. Please enter a value between 0 and 180, in increments of 10.';
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

        function resetForm() {
            document.getElementById("positionInput").value = '';
            document.getElementById("positionSlider").value = '';
            document.getElementById("weatherData").innerHTML = 'Loading weather data...';
            document.getElementById("feedbackMessage").innerText = '';
        }

        document.getElementById("positionSlider").addEventListener("input", function() {
            document.getElementById("positionInput").value = this.value;
            fetchWeather();
        });
    </script>
</body>
</html>
