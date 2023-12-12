<!-- ... (head and body tags remain unchanged) -->

<script>
    document.addEventListener("DOMContentLoaded", function() {
        // Update the input field value when the slider changes
        var positionSlider = document.getElementById("positionSlider");
        if (positionSlider) {
            positionSlider.addEventListener("input", function() {
                document.getElementById("positionInput").value = this.value;
            });
        }

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
