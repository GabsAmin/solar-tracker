<!DOCTYPE html>
<html lang="en">
<head>
    <!-- Meta tags and title remain the same as in your original code -->

    <style>
        <!-- Your CSS styles remain the same as in your original code -->
    </style>
</head>
<body>
    <h1>Servo Control with Weather</h1>
    <!-- Input fields, button, and weather container remain the same as in your original code -->

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
                    } else {
                        console.error('Failed to set servo position');
                    }
                })
                .catch(error => console.error('Error:', error));
            } else {
                console.error('Invalid position. Please enter a value between 0 and 180, in increments of 10.');
            }
        }

        document.addEventListener("DOMContentLoaded", function() {
            // Update the input field value when the slider changes
            document.getElementById("positionSlider").addEventListener("input", function() {
                document.getElementById("positionInput").value = this.value;
            });

            // Fetch weather information from OpenWeatherMap API
            function fetchWeather() {
                const apiKey = 'afa92ea67fe55fdfa48347e9f635fda6'; // Replace with your OpenWeatherMap API key
                const city = 'Barrie'; // Replace with your desired city
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
