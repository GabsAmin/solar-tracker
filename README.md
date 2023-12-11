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
            background-color: #f8f8f8;
        }
        h1, h2 {
            color: #333;
        }
        label {
            display: block;
            margin: 10px 0;
            color: #555;
        }
        input[type="number"] {
            width: 50px;
            padding: 8px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        input[type="range"] {
            width: 80%;
            margin: 0 auto;
            background-color: #eee;
            border: none;
            height: 20px;
            border-radius: 10px;
        }
        button {
            background-color: #4CAF50;
            color: white;
            padding: 12px 24px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #45a049;
        }
        #weather-container {
            margin-top: 20px;
            border: 1px solid #ccc;
            padding: 20px;
            background-color: #fff;
            border-radius: 10px;
        }
        #weather-container h2 {
            color: #333;
        }
        #weather-description, #temperature {
            color: #555;
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
            // Your existing JavaScript code
            // ...
        });
    </script>
</body>
</html>
