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
        <div class="date">{{ weather.timezone }}{{ code.code }}</div>
      </div>
      <div class="weather-box">
        <div class="temp">
          {{ Math.round(temperature) }}°

          <button
            id="celsius"
            :class="cel ? 'selected' : 'deselected'"
            @click="celsiusShow()"
          >
            C
          </button>
          <button
            id="fehren"
            :class="feh ? 'selected' : 'deselected'"
            @click="celsiusToFahrenheit()"
          >
            F
          </button>
          <button
            id="fehren"
            :class="kel ? 'selected' : 'deselected'"
            @click="celsiusToKelvin()"
          >
            K
          </button>
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
        "https://images.unsplash.com/photo-1464618663641-bbdd760ae84a?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8Mnx8d2Vic2l0ZSUyMGJhY2tncm91bmR8ZW58MHx8MHx8fDA%3D&w=1000&q=80",
      url: "https://api.open-meteo.com/v1/forecast?",
      place: "",
      cel: true,
      feh: false,
      kel: false,
      temperature: 0,
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
        { id: 7, text: "Australia", latitude: "-25.00", longitude: "135.00" },
        { id: 8, text: "Thailand", latitude: "15.50", longitude: "101.00" },
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
      this.temperature = Math.round(this.weather.current_weather.temperature);
      this.changeImage();
    },
    changeImage() {
      switch (this.code.code) {
        case 0:
          this.imageUrl =
            "https://static.vecteezy.com/system/resources/previews/003/692/649/large_2x/beautiful-clear-blue-sky-in-summer-look-lke-heaven-free-photo.jpg";
          break;
        case 1:
        case 2:
        case 3:
          this.imageUrl =
            "https://images.pexels.com/photos/96622/pexels-photo-96622.jpeg?cs=srgb&dl=pexels-photomix-company-96622.jpg&fm=jpg";
          break;
        case 45:
        case 48:
          this.imageUrl =
            "https://images.unsplash.com/photo-1486184885347-1464b5f10296?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1168&q=80";
          break;
        case 51:
        case 53:
        case 55:
          this.imageUrl =
            "https://gray-kcrg-prod.cdn.arcpublishing.com/resizer/huKOOqk_7JZ1NDAJBXbuMgyuDU4=/1200x675/smart/filters:quality(85)/cloudfront-us-east-1.images.arcpublishing.com/gray/2IK4VGMVRFJEPMLI3PODX37CSA.jpg";
          break;
        case 56:
        case 57:
          this.imageUrl =
            "https://www.langleyadvancetimes.com/wp-content/uploads/2020/02/20505369_web1_langley-weather-cloudy-cloud.jpg";
          break;
        case 61:
        case 63:
        case 65:
          this.imageUrl =
            "https://img.freepik.com/premium-photo/overcast-sky-dramatic-gray-sky-white-clouds-before-rain-rainy-season-cloudy-moody-sky_33867-2091.jpg?w=2000";
          break;
        case 66:
        case 67:
          this.imageUrl =
            "https://www.rcinet.ca/eye-on-the-arctic/wp-content/uploads/sites/30/2018/10/freezing-rain-clouds-finland.jpg";
          break;
        case 80:
        case 81:
        case 82:
          this.imageUrl =
            "https://cdn.pixabay.com/photo/2015/07/27/20/22/rain-863448_1280.jpg";
          break;
        default:
          this.imageUrl =
            "https://images.unsplash.com/photo-1496450681664-3df85efbd29f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MjB8fHdlYXRoZXJ8ZW58MHx8MHx8fDA%3D&w=1000&q=80";
          break;
      }
    },
    celsiusShow() {
      this.temperature = Math.round(this.weather.current_weather.temperature);
      this.cel = true;
      this.feh = false;
      this.kel = false;
    },
    celsiusToFahrenheit() {
      this.temperature =
        (Math.round(this.weather.current_weather.temperature) * 9) / 5 + 32;
      this.cel = false;
      this.feh = true;
      this.kel = false;
    },
    celsiusToKelvin() {
      this.temperature =
        Math.round(this.weather.current_weather.temperature) + 273.15;
      this.cel = false;
      this.feh = false;
      this.kel = true;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./Weather.css" scoped></style>
