<template>
  <div id="weather" :style="{ backgroundImage: 'url(' + imageUrl + ')' }">
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
          {{ place }}: {{ weather.latitude }}
          {{ weather.longitude }}
        </div>
        <div class="date">{{ weather.timezone }}</div>
      </div>
      <div class="weather-box">
        <div class="temp">
          {{ Math.round(weather.current_weather.temperature) }}°
        </div>
        <div class="weather">
          {{ code.description }}
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
      imageUrl:
        "https://images.pexels.com/photos/209831/pexels-photo-209831.jpeg?cs=srgb&dl=pexels-pixabay-209831.jpg&fm=jpg",
      url: "https://api.open-meteo.com/v1/forecast?",
      place: "",
      lat: "28.00",
      lon: "84.00",
      selected: "Nepal",
      places: [
        { id: 1, text: "Nepal", latitude: "28.00", longitude: "84.00" },
        { id: 2, text: "New York", latitude: "40.71", longitude: "-74.01" },
        { id: 3, text: "London", latitude: "51.51", longitude: "-0.13" },
        { id: 4, text: "India", latitude: "22.00", longitude: "79.00" },
        { id: 5, text: "California", latitude: "38.30", longitude: "-76.51" },
        { id: 6, text: "Mexico", latitude: "23.00", longitude: "-102.00" },
      ],
      weathercode: [
        { code: 0, description: "Clear Sky" },
        { code: 1, description: "Mainly clear" },
        { code: 2, description: "Partly cloudy" },
        { code: 3, description: "Overcast" },
        { code: 45, description: "Fog" },
        { code: 48, description: "Depositing rime fog" },
        { code: 51, description: "Drizzle: Light" },
        { code: 53, description: "Drizzle: Moderate" },
        { code: 55, description: "Drizzle: Dense intensity" },
        { code: 56, description: "Freezing Drizzle:" },
        { code: 57, description: "Freezing Drizzle:" },
        { code: 61, description: "Rain: Slight" },
        { code: 63, description: "Rain: Moderate" },
        { code: 65, description: "Rain: Heavy intensity" },
        { code: 66, description: "Freezing Rain: Light " },
        { code: 67, description: "Freezing Rain: Heavy intensity" },
        { code: 80, description: "Rain showers: Slight" },
        { code: 81, description: "Rain showers: moderate" },
        { code: 82, description: "Rain showers: violent" },
      ],
      weather: {},
      code: {},
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
      this.code = this.weathercode.find(
        (weatherCodes) =>
          weatherCodes.code === this.weather.current_weather.weathercode
      );
      this.changeImage();
    },
    changeImage() {
      switch (this.code.code) {
        case 0:
          this.imageUrl =
            "https://www.survivingwithandroid.com/wp-content/uploads/2014/11/android_weather_app.jpg";
          break;
        case (1, 2, 3):
          this.imageUrl =
            "https://www.survivingwithandroid.com/wp-content/uploads/2014/11/android_weather_app.jpg";
          break;
        default:
          this.imageUrl =
            "https://images.unsplash.com/photo-1496450681664-3df85efbd29f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MjB8fHdlYXRoZXJ8ZW58MHx8MHx8fDA%3D&w=1000&q=80";
          break;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./Weather.css" scoped></style>
