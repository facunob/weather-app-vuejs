<template>
  <div
    id="app"
    :class="{
      'cold-bg': weather.main.temp <= 15,
      'warm-bg': weather.main.temp > 15,
    }"
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search a city.."
          v-model="query"
          @keyup.enter="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <h2 class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </h2>
          <h3 class="date">{{ dateBuilder() }}</h3>
        </div>

        <div class="weather-box">
          <h3 class="temp">{{ Math.round(weather.main.temp) }}°C</h3>
          <h3 class="weather">{{ weather.weather[0].main }}</h3>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "5a6b350ab81c6a1c8a94c41f54f0b66f",
      api_url: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather() {
      let url = `${this.api_url}weather?q=${this.query}&units=metric&appid=${this.api_key}`;
      fetch(url)
        .then((res) => res.json())
        .then((data) => (this.weather = data));
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
  created() {
    let url = `${this.api_url}weather?q=rosario&units=metric&appid=${this.api_key}`;
    fetch(url)
      .then((res) => res.json())
      .then((data) => (this.weather = data));
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
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  width: 360px;
  margin: auto;
}
.cold-bg {
  background-image: url("./assets/cold-bg.jpg");
}
.warm-bg {
  background-image: url("./assets/warm-bg.jpg");
}
main {
  min-height: 100vh;
  padding: 25px;
  background: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-bar {
  width: 100%;
  padding: 10px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 0 16px 0 16px;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  transition: 0.4s;
}
.search-bar:focus {
  background: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0 16px 0;
  box-shadow: 0 0 12px rgba(0, 0, 0, 0.75);
}
/*  */
.location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}
.temp {
  display: inline-block;
  padding: 10px 25px;
  font-size: 75px;
  font-weight: 900;
  color: #fff;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather {
  font-size: 40px;
  color: #fff;
  font-weight: 700;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
