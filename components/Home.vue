<template lang="">
  <div class="main-container">
    <div class="text-center py-12 text-[2.5rem] loading" v-if="loading">
      Loading...
    </div>
    <DisplayWeather
      class="weather-box"
      v-if="!loading"
      :weatherData="weatherData"
      :weatherForecast="weatherForecast"
      :usersLocationWeatherData="usersLocationWeatherData"
      :usersLocationWeatherForecast="usersLocationWeatherForecast"
    />
    <Form
      class="form-box"
      :weatherData="weatherData"
      :usersLocationWeatherData="usersLocationWeatherData"
      :fetchData="fetchData"
      :fetchForecastData="fetchForecastData"
    />
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      loading: false,
      apiKey: "6d8d685969d439d8178c3b7a901ebcf4",
      weatherData: [],
      weatherForecast: [],
      usersLocationWeatherData: [],
      usersLocationWeatherForecast: [],
      lat: "",
      lng: "",
    };
  },
  methods: {
    async fetchData(city) {
      try {
        const { data } = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${this.apiKey}`
        );
        this.weatherData = data;
        this.usersLocationWeatherData = [];
      } catch (error) {
      } finally {
        this.loading = false;
      }
    },
    async fetchForecastData(city) {
      try {
        const { data } = await axios.get(
          `https://api.openweathermap.org/data/2.5/forecast/?q=${city}&units=metric&appid=${this.apiKey}`
        );
        this.weatherForecast = data;
      } catch (error) {
        console.log(error);
      } finally {
        this.loading = false;
      }
    },
    async fetchCurrentLocationWeather(position) {
      const { latitude, longitude } = position.coords;
      try {
        const { data } = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&units=metric&appid=${this.apiKey}`
        );
        this.usersLocationWeatherData = data;
      } catch (error) {
        console.log(error);
      } finally {
        this.loading = false;
      }
    },
    async fetchUsersLocationWeatherForecast(position) {
      const { latitude, longitude } = position.coords;
      try {
        const { data } = await axios.get(
          `https://api.openweathermap.org/data/2.5/forecast?lat=${latitude}&lon=${longitude}&units=metric&appid=${this.apiKey}`
        );
        this.usersLocationWeatherForecast = data;
      } catch (error) {
        console.log(error);
      } finally {
        this.loading = false;
      }
    },
    getLocationData() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          this.fetchCurrentLocationWeather
        );
      } else {
        alert("Couldn't get your location info.");
        this.loading = false;
      }
    },
    getLocationForecast() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          this.fetchUsersLocationWeatherForecast
        );
      } else {
        alert("Couldn't get your location info.");
        this.loading = false;
      }
    },
  },
  mounted() {
    this.getLocationData();
    this.getLocationForecast();
  },
};
</script>
<style lang="scss">
.main-container {
  display: flex;
  height: 100%;
  .loading {
    flex: 3;
  }
  .form-box {
    flex: 2;
    backdrop-filter: blur(15px);
    padding: 0.5rem;
    box-shadow: 0 0 0.5rem 0.1rem rgba($color: #ffffff, $alpha: 0.5);
    form {
      display: flex;
      flex-direction: column;

      width: 100%;
    }
  }
  .weather-box {
    width: 66%;
  }
}
@media screen and (max-width: 1026px) {
  .main-container {
    flex-direction: column-reverse;
    height: auto;
    .weather-box {
      width: 100vw;

      .forecast {
        padding: 0.5rem;
        margin: 0;
      }
    }
  }
}
</style>
