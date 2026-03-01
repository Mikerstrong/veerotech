<script setup>
import { ref } from 'vue'

const city = ref('Pflugerville, TX')
const weather = ref(null)
const loading = ref(false)
const error = ref('')

async function fetchWeather() {
  loading.value = true
  error.value = ''
  weather.value = null
  try {
    // US National Weather Service API for Pflugerville, TX
    const pointsUrl = 'https://api.weather.gov/points/30.4394,-97.62';
    const pointsRes = await fetch(pointsUrl);
    if (!pointsRes.ok) throw new Error('Failed to get weather grid info');
    const pointsData = await pointsRes.json();
    const forecastUrl = pointsData.properties.forecast;
    const forecastRes = await fetch(forecastUrl);
    if (!forecastRes.ok) throw new Error('Failed to get weather forecast');
    const forecastData = await forecastRes.json();
    // Get current period (first in array)
    const current = forecastData.properties.periods[0];
    weather.value = {
      description: current.shortForecast,
      temp: current.temperature,
      tempUnit: current.temperatureUnit,
      humidity: current.relativeHumidity?.value,
      wind: current.windSpeed,
      icon: current.icon,
      detailed: current.detailedForecast
    };
  } catch (e) {
    error.value = e.message;
  } finally {
    loading.value = false;
  }
}
</script>

<template>
  <div class="weather-box">
    <h2>Weather Search</h2>
    <input v-model="city" placeholder="Enter city name" @keyup.enter="fetchWeather" />
    <button @click="fetchWeather">Get Weather</button>
    <div v-if="loading">Loading...</div>
    <div v-if="error" class="error">{{ error }}</div>
    <div v-if="weather">
      <h3>Pflugerville, TX</h3>
      <p v-if="weather.description">{{ weather.description }}</p>
      <img v-if="weather.icon" :src="weather.icon" alt="Weather icon" />
      <p v-if="weather.temp">Temperature: {{ weather.temp }} {{ weather.tempUnit }}</p>
      <p v-if="weather.humidity">Humidity: {{ weather.humidity }}%</p>
      <p v-if="weather.wind">Wind: {{ weather.wind }}</p>
      <p v-if="weather.detailed">{{ weather.detailed }}</p>
    </div>
  </div>
</template>

<style scoped>
.weather-box {
  max-width: 400px;
  margin: 2em auto;
  padding: 2em;
  border-radius: 8px;
  background: #f4f4f4;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}
input {
  padding: 0.5em;
  margin-right: 0.5em;
}
button {
  padding: 0.5em 1em;
}
.error {
  color: #c00;
  margin-top: 1em;
}
</style>
