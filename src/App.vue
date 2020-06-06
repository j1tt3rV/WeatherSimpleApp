<template>
  <div id="app" :class="typeof weatherCarrier.main != 'undefined' && weatherCarrier.main.temp > 20 ? 'warm' : ''">
    <main>
      <div class="searchBox">
        <input
          type="text"
          class="searchBar"
          placeholder="Search..."
          v-model="querySearchWeather"
          v-on:keypress="fetchWeather"
        />
      </div>
      <div class="weatherWrap" v-if="typeof weatherCarrier.main != 'undefined'">
        <div class="locationBox">
          <div class="location">
            {{ weatherCarrier.name }}, {{ weatherCarrier.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weatherBox">
          <div class="temperature">
            {{ Math.round(weatherCarrier.main.temp) }}°C
          </div>
          <div class="weatherType">{{ weatherCarrier.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
//v-bind a two way binding
export default {
  name: "App",
  data() {
    return {
      apiKey: "4977c05c1d87f1ef3d6db1a0ba3d223c",
      apiBaseUrl: "https://api.openweathermap.org/data/2.5/",
      querySearchWeather: "",
      weatherCarrier: {},
    };
  },
  methods: {
    fetchWeather(eventFetchWeather) {
      if (eventFetchWeather.key == "Enter") {
        fetch(
          `${this.apiBaseUrl}weather?q=${this.querySearchWeather}&units=metric&APPID=${this.apiKey}`
        )
          .then((responseOfWeatherApi) => {
            return responseOfWeatherApi.json();
          })
          .then(this.setResults);
      }
    },
    setResults(resultsWeather) {
      this.weatherCarrier = resultsWeather;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat", sans-serif;
}
#app {
  background-image: url("./assets/cold-bg.png");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.warm {
  background-image: url("./assets/hot-bg.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.searchBox {
  width: 100%;
  margin-bottom: 30px;
}

.searchBox .searchBar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 16px 0px 16px 0px;
  transition: 0.4s;
}

.searchBox .searchBar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 0px 16px 0px 16px;
}

.locationBox .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.locationBox .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weatherBox {
  text-align: center;
}
.weatherBox .temperature {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weatherBox .weatherType {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(255, 255, 255, 0.25);
}
</style>
