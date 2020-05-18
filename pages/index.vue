<template>
  <div>
    <p>
      Dane pogodowe oraz ikony pożyczone z
      <a href="https://openweathermap.org/">OpenWeatherMap</a>
      (<a
        href="https://samples.openweathermap.org/data/2.5/forecast/hourly?id=524901&appid=439d4b804bc8187953eb36d2a8c26a02"
        >plik</a
      >)
    </p>
    <weather-widget />
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import WeatherWidget from '~/components/WeatherWidget.vue';

export default Vue.extend({
  components: {
    WeatherWidget
  }
});
</script>
