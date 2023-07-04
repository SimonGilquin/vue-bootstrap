<template>
  <div class="weather">
    <h1>Weather data</h1>
    <div v-if="loading" class="loading">
      Loading... Please refresh once the ASP.NET backend has started. See
      <a href="https://aka.ms/jspsintegrationvue">https://aka.ms/jspsintegrationvue</a> for more
      details.
    </div>

    <div v-if="forecastData?.length > 0" class="content">
      <table>
        <thead>
          <tr>
            <th>Date</th>
            <th>Temp. (C)</th>
            <th>Temp. (F)</th>
            <th>Summary</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="forecast in forecastData" :key="forecast.date">
            <td>{{ forecast.date }}</td>
            <td>{{ forecast.temperatureC }}</td>
            <td>{{ forecast.temperatureF }}</td>
            <td>{{ forecast.summary }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { onMounted, ref } from 'vue'

const loading = ref(true)
const forecastData = ref<
  { date: string; temperatureC: number; temperatureF: number; summary: string }[]
>([])

onMounted(() => {
  fetchData()
})

function fetchData() {
  forecastData.value = []
  loading.value = true

  fetch('https://localhost:7076/api/weatherforecast')
    .then((r) => r.json())
    .then((json) => {
      forecastData.value = json
      loading.value = false
      return
    })
}
</script>

<style>
@media (min-width: 1024px) {
  .weather {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
