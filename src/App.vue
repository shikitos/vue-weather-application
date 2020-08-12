<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : '' ">
      <main>
          <div class="search-box">
              <label for="search-bar">
                  <input
                      type="text"
                      id="search-bar"
                      class="search-bar"
                      placeholder="Search..."
                      v-model="query"
                      @keyup.enter="fetchWeather"
                  >
              </label>
          </div>
          <div class="weather-wrap">
              <div class="location-box">
                  <div class="location">
                      {{ weather.name }}, {{ weather.sys.country }}
                  </div>
                  <div class="date">
                    {{ dateBuilder() }}
                  </div>
              </div>
              <div class="weather-box" v-if="typeof weather.main != 'undefined'">
                  <div class="temp">{{ Math.round(weather.main.temp) }}°</div>
                  <div class="weather">{{ weather.weather[0].main }}</div>
              </div>
          </div>
      </main>
  </div>
</template>

<script>
export default {
  name: 'App',
    data() {
      return {
          api_key: '1829cf122d4a569ad61a417650ce77a7',
          url_base: 'https://api.openweathermap.org/data/2.5/',
          query: '',
          weather: {}
      }
    },
    methods: {
        fetchWeather() {
                fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
                    .then(res => {
                        this.query = '';
                        return res.json();
                    }).then(this.setResults);
        },
        setResults(results) {
            this.weather = results;
        },
        dateBuilder() {
            let date = new Date();
            let months = [
                'January',
                'February',
                'March',
                'April',
                'May',
                'June',
                'July',
                'August',
                'September',
                'October',
                'November',
                'December'
            ];
            let days = [
                'Sunday',
                'Monday',
                'Tuesday',
                'Wednesday',
                'Thursday',
                'Friday',
                'Saturday'
            ];

            let day = days[date.getDay()];
            let fullDate = date.getDate();
            let month = months[date.getMonth()];
            let year = date.getFullYear();

            return `${day} ${fullDate} ${month} ${year}`
        }
    }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
body {
    font-family: Montserrat, sans-serif;
}
#app {
    background-image: url("./assets/cold-bg.jpg");
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
}
main {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}
#app.warm {
    background-image: url("./assets/warm-bg.jpg");
}
.search-box {
    width: 100%;
    margin-bottom: 30px;
}
.search-box .search-bar {
    display: block;
    width: 100%;
    padding: 15px;
    color: #313131;
    font-size: 20px;
    appearance: none;
    border: none;
    outline: none;
    background: rgba(255, 255, 255, 0.15) none;
    border-radius: 0 16px 0 16px;
    transition: 0.4s;
}
.search-box .search-bar:focus {
    box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.75);
    border-radius: 16px 0 16px 0;
}
.location-box .location {
    color: #fff;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
    color: #fff;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
}
.weather-box {
    text-align: center;
}
.weather-box .temp {
    display: inline-block;
    padding: 10px 25px;
    color: #fff;
    font-size: 102px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    margin: 30px 0;
    border-radius: 16px;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
    color: #fff;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
