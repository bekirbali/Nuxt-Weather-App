<template lang="">
  <div class="display-container">
    <div v-if="!this.weatherData.name" class="main-container">
      <div class="main-details">
        <div class="degree">
          <p>{{ Math.round(this.usersLocationWeatherData?.main?.temp) }}°</p>
        </div>
        <div class="city-info">
          <div class="city">
            {{ this.usersLocationWeatherData?.name }}
          </div>

          <div class="date">
            <p>
              {{
                new Date(
                  this.usersLocationWeatherData?.dt * 1000
                ).toLocaleDateString()
              }}
            </p>
            <div class="day">
              {{
                new Date(
                  this.usersLocationWeatherData?.dt * 1000
                ).toLocaleDateString("en-US", {
                  weekday: "long",
                })
              }}
            </div>
          </div>
        </div>
        <div class="weather-info">
          <div class="icon">
            <img
              :src="`../icons/${this.usersLocationWeatherData?.weather?.[0]?.icon}.svg`"
              alt=""
            />
          </div>
          <div class="weather-name">
            {{ this.usersLocationWeatherData?.weather?.[0]?.main }}
          </div>
        </div>
      </div>
    </div>
    <div v-if="this.weatherData.name" class="main-container">
      <div class="main-details">
        <div class="degree">
          <p>{{ Math.round(this.weatherData?.main?.temp) }}°</p>
        </div>
        <div class="city-info">
          <div class="city">{{ this.weatherData?.name }}</div>

          <div class="date">
            <p>
              {{ new Date(this.weatherData?.dt * 1000).toLocaleDateString() }}
            </p>
            <div class="day">
              {{
                new Date(this.weatherData?.dt * 1000).toLocaleDateString(
                  "en-US",
                  {
                    weekday: "long",
                  }
                )
              }}
            </div>
          </div>
        </div>
        <div class="weather-info">
          <div class="icon">
            <img
              :src="`../icons/${this.weatherData?.weather?.[0]?.icon}.svg`"
              alt=""
            />
          </div>
          <div class="weather-name">
            {{ this.weatherData?.weather?.[0]?.main }}
          </div>
        </div>
      </div>
    </div>
    <div v-if="!this.weatherData.name" class="forecast">
      <div v-for="day in usersLocationWeatherForecast.list" class="days">
        <p>
          {{
            new Date(day.dt * 1000).toLocaleDateString(en - US, {
              weekday: "long",
              hour: "numeric",
            })
          }}
        </p>
        <!-- <p>{{ day.dt_txt }}</p> -->
        <img :src="`../icons/${day?.weather[0].icon}.svg`" alt="" />
        <p>
          {{
            day?.weather[0].description.replace(/\b\w/g, (c) => c.toUpperCase())
          }}
        </p>
      </div>
    </div>
    <div v-if="this.weatherData.name" class="forecast">
      <div v-for="day in weatherForecast.list" class="days">
        <p>
          {{
            new Date(day.dt * 1000).toLocaleDateString(en - US, {
              weekday: "long",
              hour: "numeric",
            })
          }}
        </p>
        <!-- <p>{{ day.dt_txt }}</p> -->
        <img :src="`../icons/${day?.weather[0].icon}.svg`" alt="" />
        <p>
          {{
            day?.weather[0].description.replace(/\b\w/g, (c) => c.toUpperCase())
          }}
        </p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    weatherData: Object,
    weatherForecast: Object,
    usersLocationWeatherData: Object,
    usersLocationWeatherForecast: Object,
  },
};
</script>
<style lang="scss" scoped>
.display-container {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  /* height: 100%; */

  .forecast {
    display: flex;
    overflow-x: auto;
    gap: 1rem;
    backdrop-filter: blur(10px);
    margin-right: 0.5rem;
    color: white;

    .days {
      padding: 1rem;
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
      justify-content: center;
      img {
        width: 6rem;
      }
      p {
        width: 8rem;
      }
    }
    &::-webkit-scrollbar {
      width: 0;
      height: 0;
      transition: all 0.5s ease-in-out;
    }
    &:hover::-webkit-scrollbar {
      height: 15px;
    }

    &::-webkit-scrollbar-thumb {
      background: #000;
      border-radius: 10px;
    }

    &::-webkit-scrollbar-track {
      background: #9b3838;
      border-radius: 0.5rem;
    }
    &::-webkit-scrollbar-corner {
      background: #9b3838;
      border-radius: 0.3rem;
    }
  }
  .main-container {
    display: flex;
    align-items: center;
    padding-left: 1rem;
    /* height: 15rem; */
    .main-details {
      display: flex;
      gap: 2rem;
      .degree {
        display: flex;
        p {
          font-size: 6rem;
          padding: 0;
        }
      }
      .city-info {
        display: flex;
        flex-direction: column;
        gap: 0.5rem;
        .city {
          font-size: 2.5rem;
        }
        .date {
          font-size: 1.3rem;
          display: flex;
          flex-direction: column;
          gap: 0.5rem;
        }
      }
      .weather-info {
        .icon {
          font-size: 1.2rem;
          img {
            border-radius: 50%;
            transition: 0.3s ease-in-out;
            &:hover {
              transform: scale(1.3);
            }
          }
        }
        .weather-name {
          font-size: 2.2rem;
          text-align: center;
        }
      }
    }
  }
  &::before {
    position: absolute;
    content: "";
    left: 0;
    right: 0;
    width: 100vw;
    height: 100vh;
    background-image: linear-gradient(
      to right,
      rgba(0, 0, 0, 0.068),
      rgba(0, 0, 0, 0.315)
    );
    z-index: -1;
  }
}
@media screen and (max-width: 1026px) {
  .display-container {
    width: 100vw;
    height: auto;
    justify-content: space-evenly;
  }
  .main-container {
    height: auto;
    .main-details {
      width: 300px;
      height: 400px;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 0 !important;
      text-align: center;
      .degree {
        p {
          font-size: 3rem !important;
        }
      }
      img {
        width: 100px;
        height: 100px;
      }

      .city-info {
        .city {
          font-size: 1rem;
        }
        .date {
          p {
            display: none;
          }
        }
      }
    }
  }
}
</style>
