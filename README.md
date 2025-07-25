# SkyCast - Find My Weather
## Date: 25.07.2025
## Objective:
To build a responsive single-page application using React that allows users to enter a city name and retrieve real-time weather information using the OpenWeatherMap API. This project demonstrates the use of Axios for API calls, React Router for navigation, React Hooks for state management, controlled components with validation, and basic styling with CSS.
## Tasks:

#### 1. Project Setup
Initialize React app.

Install necessary dependencies: npm install axios react-router-dom

#### 2. Routing
Set up BrowserRouter in App.js.

Create two routes:

/ – Home page with input form.

/weather – Page to display weather results.

#### 3. Home Page (City Input)
Create a controlled input field for the city name.

Add validation to ensure the input is not empty.

On valid form submission, navigate to /weather and store the city name.

#### 4. Weather Page (API Integration)
Use Axios to fetch data from the OpenWeatherMap API using the city name.

Show temperature, humidity, wind speed, and weather condition.

Convert and display temperature in both Celsius and Fahrenheit using useMemo.

#### 5. React Hooks
Use useState for managing city, weather data, and loading state.

Use useEffect to trigger the Axios call on page load.

Use useCallback to optimize form submit handler.

Use useMemo for temperature conversion logic.

#### 6. UI Styling (CSS)
Create a responsive and clean layout using CSS.

Style form, buttons, weather display cards, and navigation links.

## Programs:

##Home.js:
```
import React, { useState } from "react";
import { useNavigate } from "react-router-dom";

function Home() {
  const [city, setCity] = useState("");
  const [error, setError] = useState("");
  const navigate = useNavigate();

  const handleSubmit = (e) => {
    e.preventDefault();

    if (city === "") {
      setError("Please enter a city name");
    } else {
      setError("");
      sessionStorage.setItem("city", city);
      navigate("/weather");
    }
  };

  return (
    <div className="container">
      <h1>SkyCast</h1>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={city}
          placeholder="Enter City Name"
          onChange={(e) => setCity(e.target.value)}
        />
        <button type="submit">Check Weather</button>
      </form>
      {error && <p className="error">{error}</p>}
    </div>
  );
}

export default Home;
```
##Weather.js
```import React, { useState, useEffect } from "react";
import { useNavigate } from "react-router-dom";
import axios from "axios";

function Weather() {
  const [weatherData, setWeatherData] = useState(null);
  const [error, setError] = useState("");
  const navigate = useNavigate();

  const city = sessionStorage.getItem("city");

  useEffect(() => {
    if (!city) {
      navigate("/");
      return;
    }

    const API_KEY = "274f95ad3cfc436484e13611251207"; 
    axios
      .get(`https://api.weatherapi.com/v1/current.json?key=${API_KEY}&q=${city}`)
      .then((res) => {
        setWeatherData(res.data);
        setError("");
      })
      .catch((err) => {
        setError("Could not fetch weather. Please check the city name or try again later.");
        setWeatherData(null);
      });
  }, [city, navigate]);

  return (
    <div className="container">
      <h1>Weather Report</h1>

      {error && <p className="error">{error}</p>}

      {weatherData && (
        <div className="card">
          <h2>
            {weatherData.location.name}, {weatherData.location.country}
          </h2>
          <p><strong>Condition:</strong> {weatherData.current.condition.text}</p>
          
          <p><strong>Temperature:</strong> {weatherData.current.temp_c}°C / {weatherData.current.temp_f}°F</p>
          <p><strong>Humidity:</strong> {weatherData.current.humidity}%</p>
          <p><strong>Wind Speed:</strong> {weatherData.current.wind_kph} km/h</p>
        </div>
      )}

      <button onClick={() => navigate("/")}>Go Back</button>
    </div>
  );
}

export default Weather;
```
##App.css
```
.container {
  max-width: 400px;
  margin: 40px auto;
  padding: 20px;
  background-color: #f0f8ff; 
  border-radius: 10px;
  text-align: center;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  font-family: Arial, sans-serif;
}

.card {
  background-color: white;
  border-radius: 8px;
  padding: 15px;
  margin-top: 20px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

img {
  width: 60px;
  height: 60px;
  margin: 10px 0;
}

button {
  margin-top: 25px;
  padding: 10px 25px;
  background-color: #1e90ff; 
  border: none;
  border-radius: 6px;
  color: white;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #0c6cd9;
}

.error {
  color: red;
  margin-top: 15px;
  font-weight: bold;
  font-size: 0.9rem;
}
```
##App.js
```
import React from "react";
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Home from "./pages/Home";
import Weather from "./pages/Weather";
import './App.css';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/weather" element={<Weather />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

## Output:

<img width="1920" height="969" alt="image" src="https://github.com/user-attachments/assets/3f5794b6-30c3-469a-a110-fe642bc885b8" />

<img width="1920" height="1019" alt="image" src="https://github.com/user-attachments/assets/fec88afe-18fa-4a0f-8fe0-a0b29c4268bb" />

## Result:
A responsive single-page application using React that allows users to enter a city name and retrieve real-time weather information using the OpenWeatherMap API has been built successfully. 
