<template>
  <div class="weather">
    <div class="search-box">
      <select v-model="selected">
        <option v-for="place in places" v-bind:key="place.id">
          {{ place.text }}
        </option>
      </select>

      <button @click="fetchWeather" type="button" name="button">Display</button>
    </div>
    <div
      class="weather-wrap"
      v-if="typeof weather.current_weather != 'undefined'"
    >
      <div class="location-box">
        <div class="location">
          {{ place }}:<br />
          {{ weather.latitude }} {{ weather.longitude }}
        </div>
        <div class="date">{{ weather.timezone }}</div>
      </div>
      <div class="weather-box">
        <div class="temp">
          {{ Math.round(weather.current_weather.temperature) }}°
        </div>
        <div class="weather">
          {{ weather.current_weather.temperature }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      api: "",
      url: "https://api.open-meteo.com/v1/forecast?",
      place: "",
      lat: "28.00",
      lon: "84.00",
      selected: "Nepal",
      places: [
        { id: 1, text: "Nepal", latitude: "28.00", longitude: "84.00" },
        { id: 2, text: "New York", latitude: "40.71", longitude: "-74.01" },
        { id: 3, text: "London", latitude: "51.51", longitude: "-0.13" },
      ],
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e) {
        const place = this.places.find((place) => place.text === this.selected);
        this.lat = place.latitude;
        this.lon = place.longitude;
        this.place = place.text;
        fetch(
          `${this.url}latitude=${this.lat}&longitude=${this.lon}&current_weather=true`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./Weather.css" scoped></style>
