<template>
  <!-- MAIN -->
  <main :class="sunny">
    <figure id="back-home">
      <router-link to="/" @click="backHome()">
        <img src="@/assets/icons/arrow.png" alt="Back Home" title="Back Home" />
      </router-link>
    </figure>
    <!-- HEADER -->
    <header>
      <h1>{{ nameCity }}</h1>
      <h4>{{ condition }}</h4>
    </header>
    <!-- CENTER CONTENT -->
    <div class="wrapper">
      <div class="temp">
        <h5>{{ temp }}<span>°C</span></h5>
        <div class="maxmin-wrapper">
          <div class="max">
            <img src="@/assets/icons/temp-arrow.png" />
            <p>{{ maxTemp }}<span>°</span></p>
          </div>
          <div class="min">
            <img src="@/assets/icons/temp-arrow.png" />
            <p>{{ minTemp }}<span>°</span></p>
          </div>
        </div>
      </div>
      <figure>
        <img :src="conditionIcon" alt="Sunny" title="Sunny" />
      </figure>
    </div>
    <div class="info">
      <div class="item">
        <h6>dawn</h6>
        <figure>
          <img :src="dawnIcon" alt="Dawn" title="Dawn" />
        </figure>
        <p>{{ dawn }}°C</p>
      </div>
      <div class="item">
        <h6>morning</h6>
        <figure>
          <img :src="morningIcon" alt="Morning" title="Morning" />
        </figure>
        <p>{{ morning }}°C</p>
      </div>
      <div class="item">
        <h6>afternoon</h6>
        <figure>
          <img :src="afternoonIcon" alt="Afternoon" title="Afternoon" />
        </figure>
        <p>{{ afternoon }}°C</p>
      </div>
      <div class="item">
        <h6>night</h6>
        <figure>
          <img :src="nightIcon" alt="Night" title="Night" />
        </figure>
        <p>{{ night }}°C</p>
      </div>
    </div>
    <!-- FOOTER -->
    <footer>
      <div class="wind-speed">
        <h6>wind speed</h6>
        <p>{{ windSpeed }} m/s</p>
      </div>
      <div class="sunrise">
        <h6>sunrise</h6>
        <p>{{ sunrise }}</p>
      </div>
      <div class="sunset">
        <h6>sunset</h6>
        <p>{{ sunset }}</p>
      </div>
      <div class="humidity">
        <h6>humidity</h6>
        <p>{{ humidity }}%</p>
      </div>
    </footer>
  </main>
  <!-- MODAL -->
  <div id="modal">
    <!-- HEADER -->
    <header>
      <h1>Weather</h1>
      <h4>select a city</h4>
    </header>
    <!-- CENTER CONTENT -->
    <div class="wrapper">
      <figure>
        <img src="@/assets/icons/earth.png" alt="Earth" title="Earth" />
      </figure>
    </div>
    <!-- FOOTER -->
    <footer>
      <button class="link" @click="dallol(), close()">
        Dallol
      </button>
      <button class="link" @click="fairbanks(), close()">
        Fairbanks
      </button>
      <button class="link" @click="londres(), close()">
        Londres
      </button>
      <button class="link" @click="recife(), close()">
        Recife
      </button>
      <button class="link" @click="vancouver(), close()">
        Vancouver
      </button>
      <button class="link" @click="yakutsk(), close()">
        Yakutsk
      </button>
    </footer>
  </div>
</template>

<script>
// import gsap from 'gsap';
export default {
  name: 'Home',
  data() {
    return {
      currentCity: '',
      apiKey: '6b121f01987241a8b45195128212408',
      nameCity: '',
      condition: '',
      conditionIcon: '',
      temp: '',
      minTemp: '',
      maxTemp: '',
      dawn: '',
      dawnIcon: '',
      morning: '',
      morningIcon: '',
      afternoon: '',
      afternoonIcon: '',
      night: '',
      nightIcon: '',
      windSpeed: '',
      sunrise: '',
      sunset: '',
      humidity: '',
      currentCondition: '',
    };
  },
  watch: {
    currentCity(valor) {
      fetch(
        `http://api.weatherapi.com/v1/forecast.json?key=${this.apiKey}&q=${valor}&days=1&aqi=no&alerts=no`,
      )
        .then((response) => response.json())
        .then((response) => {
          this.nameCity = response.location.name;
          this.condition = response.current.condition.text;
          this.conditionIcon = 'https:' + response.current.condition.icon;
          this.temp = response.current.temp_c.toFixed(0);
          this.minTemp = response.forecast.forecastday[0].day.mintemp_c.toFixed(
            0,
          );
          this.maxTemp = response.forecast.forecastday[0].day.maxtemp_c.toFixed(
            0,
          );
          // this.maxTemp = response.forecast[0].day.maxtemp_c;
          this.dawn = response.forecast.forecastday[0].hour[3].temp_c;
          this.dawnIcon =
            'https:' + response.forecast.forecastday[0].hour[3].condition.icon;
          this.morning = response.forecast.forecastday[0].hour[9].temp_c;
          this.morningIcon =
            'https:' + response.forecast.forecastday[0].hour[9].condition.icon;
          this.afternoon = response.forecast.forecastday[0].hour[15].temp_c;
          this.afternoonIcon =
            'https:' + response.forecast.forecastday[0].hour[15].condition.icon;
          this.night = response.forecast.forecastday[0].hour[21].temp_c;
          this.nightIcon =
            'https:' + response.forecast.forecastday[0].hour[21].condition.icon;
          this.windSpeed = (response.current.wind_mph / 3.6).toFixed(2);
          this.sunrise = response.forecast.forecastday[0].astro.sunrise;
          this.sunset = response.forecast.forecastday[0].astro.sunset;
          this.humidity = response.current.humidity;
          this.currentCondition = this.condition;
          var main = document.querySelector('main');
          if (
            this.currentCondition === 'Sunny' ||
            this.currentCondition === 'Clear'
          ) {
            main.style.background =
              'linear-gradient(to bottom, #58cbdb 50%, #3a9eb0)';
            main.style.color = '#fff';
          } else if (
            this.currentCondition === 'Partially cloudy' ||
            this.currentCondition === 'Partly cloudy' ||
            this.currentCondition === 'Cloudy' ||
            this.currentCondition === 'Overcast' ||
            this.currentCondition === 'Rain' ||
            this.currentCondition === 'Rainy' ||
            this.currentCondition === 'Stormy' ||
            this.currentCondition === 'Light rain'
          ) {
            main.style.background =
              'linear-gradient(to bottom, #626a78 50%, #3d4454)';
            main.style.color = '#fff';
          } else if (
            this.currentCondition === 'Drizzle' ||
            this.currentCondition === 'Snow' ||
            this.currentCondition === 'Snowy'
          ) {
            main.style.background =
              'linear-gradient(to bottom, #e0e0e0 50%, #a6a6a6)';
            main.style.color = '#333';
          }
        });
    },
  },
  methods: {
    dallol() {
      this.currentCity = 'Dallol';
    },
    fairbanks() {
      this.currentCity = 'Fairbanks';
    },
    londres() {
      this.currentCity = 'Londres';
    },
    recife() {
      this.currentCity = 'Recife';
    },
    vancouver() {
      this.currentCity = 'Vancouver';
    },
    yakutsk() {
      this.currentCity = 'Yakutsk';
    },
    close() {
      document.querySelector('#modal').style.display = 'none';
    },
    backHome() {
      document.location.reload(true);
    },
  },
};
</script>

<style lang="scss">
/* VARIABLES */
$sunny: linear-gradient(to bottom, #58cbdb, #3a9eb0);
$rainy: linear-gradient(to bottom, #626a78, #3d4454);
$snowy: linear-gradient(to bottom, #e0e0e0, #a6a6a6);
$mobile: 480px;
$desktop: 1024px;
/* MIXINS RESPONSIVO */
@mixin desktop {
  @media only screen and (max-width: $desktop) {
    @content;
  }
}
@mixin mobile {
  @media only screen and (max-width: $mobile) {
    @content;
  }
}
/* MAIN */
main {
  background: #333;
  #back-home {
    position: absolute;
    left: 2%;
    top: 4%;
    @include mobile {
      left: 5%;
      top: 4%;
    }
    img {
      transform: rotate(180deg);
      width: 1.4rem;
      filter: brightness(10);
      @include mobile {
        width: 1.2rem;
      }
    }
  }
  /* CENTER CONTENT */
  .wrapper {
    .temp {
      margin: 1rem 0 0 0;
      text-align: center;
      position: relative;
      h5 {
        font-size: 7rem;
        font-weight: 300;
        line-height: 100%;
        position: relative;
        @include mobile {
          font-size: 6rem;
        }
        span {
          font-size: 1.4rem;
          line-height: 210%;
          position: absolute;
          right: -2rem;
          @include mobile {
            line-height: 340%;
            right: -1.5rem;
            font-size: 0.8rem;
          }
        }
      }
      .maxmin-wrapper {
        position: absolute;
        right: -2.6rem;
        bottom: 1rem;
        @include mobile {
          right: -2.3rem;
          bottom: 1.4rem;
        }
        .max,
        .min {
          display: flex;
          align-items: center;
          img {
            width: 1rem;
            @include mobile {
              width: 0.9rem;
            }
          }
          p {
            font-weight: 100;
            @include mobile {
              font-size: 0.8rem;
            }
          }
        }
        .max {
          img {
            transform: rotate(-90deg);
          }
        }
        .min {
          img {
            transform: rotate(90deg);
          }
        }
      }
    }
    figure {
      margin: 0.8rem 0 2.4rem 0;
      text-align: center;
      @include mobile {
        margin: 0.4rem 0 1rem 0;
      }
    }
  }
  /* INFO */
  .info {
    display: flex;
    justify-content: space-between;
    width: 30%;
    margin: 0 0 2.4rem 0;
    text-align: center;
    @include desktop {
      width: 80%;
    }
    @include mobile {
      width: 100%;
    }
    .item {
      letter-spacing: 0.1rem;
      width: 25%;
      h6 {
        font-weight: 200;
        font-size: 0.9rem;
      }
      figure {
        margin: 0.6rem 0;
        img {
          width: 2rem;
        }
      }
      p {
        font-weight: 200;
        font-size: 1.3rem;
        @include mobile {
          font-size: 0.9rem;
        }
      }
    }
  }
  /* FOOTER */
  footer {
    @include mobile {
      padding: 0 2rem;
    }
    div {
      letter-spacing: 0.1rem;
      @include mobile {
        letter-spacing: 0;
      }
      + div {
        border-left: 0.1rem solid #fff;
        padding: 0 0 0 1.5rem;
        @include mobile {
          padding: 0 0 0 0.5rem;
        }
      }
      h6 {
        font-weight: 100;
        font-size: 0.9rem;
        @include mobile {
          font-size: 0.8rem;
        }
      }
      p {
        font-weight: 200;
        font-size: 0.9rem;
        margin: 0.6rem 0 0 0;
        @include mobile {
          font-size: 0.8rem;
        }
      }
    }
  }
}
/* MODAL */
#modal {
  background: #0f0f0f;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  color: #fff;
  /* FOOTER */
  footer {
    .link {
      width: 33%;
      color: #fff;
      font-size: 1.35rem;
      font-weight: 200;
      letter-spacing: 0.05rem;
      transition: 0.2s;
      border-radius: 0.2rem;
      background: transparent;
      border: none;
      &:hover {
        background: rgba(#fff, 0.02);
      }
      @include mobile {
        font-size: 1.2rem;
      }
    }
  }
}
</style>
