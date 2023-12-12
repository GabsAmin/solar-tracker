function getWeather() {
    const apiKey = 'afa92ea67fe55fdfa48347e9f635fda6';
    const city = 'Barrie';

    const weatherApiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}`;

    fetch(weatherApiUrl)
        .then(response => response.json())
        .then(data => {
            const temperature = data.main.temp;
            const condition = data.weather[0].description;

            const weatherDataElement = document.getElementById('weatherData');
            weatherDataElement.innerHTML = `Temperature: ${temperature} K, Condition: ${condition}`;
        })
        .catch(error => {
            console.error('Error fetching weather data:', error);
            const weatherDataElement = document.getElementById('weatherData');
            weatherDataElement.innerHTML = 'Failed to fetch weather data.';
        });
}
