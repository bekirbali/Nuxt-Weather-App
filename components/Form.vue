<template lang="">
  <div>
    <form @submit="submitHandler">
      <div class="search-bar">
        <input
          type="text"
          name="search"
          id="search"
          @change="searchText"
          v-model="search"
          placeholder="Search a city"
        />
        <button type="submit">
          <Icon class="icon" name="simple-line-icons:magnifier" />
        </button>
      </div>
      <div class="suggest-city">
        <ul class="cities" @click="suggestedSearch">
          <button v-for="city in suggested" class="city-btn">{{ city }}</button>
        </ul>
      </div>
      <div v-if="!this.weatherData?.name" class="weather-details">
        <h2>Weather Details of {{ this.usersLocationWeatherData?.name }}</h2>
        <ul class="details">
          <li class="weather-description">
            <p class="name">Details</p>
            <p class="weather-detail">
              {{
                this.usersLocationWeatherData?.weather?.[0]?.description
                  .split(" ")
                  .map((word) => word[0].toUpperCase() + word.slice(1))
                  .join(" ")
              }}
            </p>
          </li>
          <li>
            <p class="name">Humidity</p>
            <p class="humidity">
              {{ this.usersLocationWeatherData?.main?.humidity }}%
            </p>
          </li>
          <li>
            <p class="name">Wind</p>
            <p class="wind">
              {{ this.usersLocationWeatherData?.wind?.speed }} km/h
            </p>
          </li>
        </ul>
      </div>
      <div v-if="this.weatherData?.name" class="weather-details">
        <h2>Weather Details of {{ this.weatherData?.name }}</h2>
        <ul class="details">
          <li class="weather-description">
            <p class="name">Details</p>
            <p class="weather-detail">
              {{
                this.weatherData?.weather?.[0]?.description
                  .split(" ")
                  .map((word) => word[0].toUpperCase() + word.slice(1))
                  .join(" ")
              }}
            </p>
          </li>
          <li>
            <p class="name">Humidity</p>
            <p class="humidity">{{ this.weatherData?.main?.humidity }}%</p>
          </li>
          <li>
            <p class="name">Wind</p>
            <p class="wind">{{ this.weatherData?.wind?.speed }} km/h</p>
          </li>
        </ul>
      </div>
    </form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      search: "",
      suggested: ["Toronto", "Ottawa", "Hawaii", "Oslo"],
    };
  },
  props: {
    fetchData: Function,
    fetchForecastData: Function,
    weatherData: Object,
    usersLocationWeatherData: Object,
  },
  methods: {
    suggestedSearch(e) {
      e.preventDefault();
      e.target.className.includes("city-btn") &&
        this.fetchData(e.target.innerText);
      e.target.className.includes("city-btn") &&
        this.fetchForecastData(e.target.innerText);
    },
    submitHandler(e) {
      e.preventDefault();
      if (!this.search) return;
      if (
        this.weatherData?.name
          ?.toLowerCase()
          .includes(this.search.toLowerCase())
      ) {
        alert(`You are already seeing the weather of ${this.search}`);
        console.log("hi");
        return;
      }
      if (
        this.usersLocationWeatherData?.name
          ?.toLowerCase()
          .includes(this.search.toLowerCase())
      ) {
        alert(`You are already seeing the weather of ${this.search}`);
        console.log("hi");
        return;
      }
      this.fetchData(this.search);
      this.fetchForecastData(this.search);
      this.suggested.pop();
      this.suggested.unshift(
        this.search
          .split(" ")
          .map((word) => word[0].toUpperCase() + word.slice(1))
          .join(" ")
      );
      this.search = "";
    },
    searchText(e) {
      this.search = e.target.value.toLowerCase();
    },
  },
};
</script>
<style lang="scss" scoped>
form {
  height: 100%;
  .search-bar {
    display: flex;
    gap: 0.5rem;
    border: none !important;
    input {
      color: white;
      flex: 10;
      height: 2.5rem;
      background: none;
      border: none;
      border-bottom: 1px solid white;
      font-size: 1.5rem;
      padding: 0.2rem 0.5rem 0.2rem 0.2rem;
      margin-top: 1rem;
      text-transform: capitalize;
      &::placeholder {
        color: rgba(255, 255, 255, 0.753);
        padding: 0.3rem;
      }
      &:focus {
        transition: 1s ease;
        outline: none;
        border-bottom: 1px solid orangered;
        background: none;
      }
    }
    button {
      flex: 2;
      width: 14%;
      display: flex;
      align-items: center;
      justify-content: center;
      border: none;
      background: none;
      &:hover {
        cursor: pointer;
      }
      .icon {
        color: white;
        font-weight: 700;
      }
    }
  }
  .suggest-city {
    flex: 1;
    padding: 0.5rem;
    border-bottom: 1px solid white;
    display: flex;

    ul.cities {
      height: 100%;
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      color: white;
      justify-content: space-between;
    }
  }
  .weather-details {
    color: white;
    margin-top: 1rem;
    flex: 2;
    display: flex;
    flex-direction: column;
    justify-content: center;
    h2 {
      font-size: 1.5rem;
      font-weight: 500;
    }
    ul.details {
      display: flex;
      flex-direction: column;
      justify-content: space-around;
      /* border: 2px solid red; */
      height: 100%;
      color: white;
      li {
        display: flex;
        border-bottom: 1px dashed white;
        justify-content: space-between;
        padding-right: 0.5rem;
      }
    }
  }
}
input:-webkit-autofill,
input:-webkit-autofill:hover,
input:-webkit-autofill:focus input:-webkit-autofill {
  -webkit-text-fill-color: rgb(255, 255, 255) !important;
  -webkit-box-shadow: 0 0 0px 0px #28292d inset !important;
  transition: background-color 5000s ease-in-out 0s !important;
}
:-internal-autofill-previewed {
  font-size: 22px !important;
}
</style>
