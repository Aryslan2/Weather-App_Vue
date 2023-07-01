<script>
import axios from "axios"

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    cityName() {
      return "<" + this.city + ">"
    },

    showTemp() {
      return "Temperature: " + this.info.main.temp + "℃"
    },

    showFeelsLike() {
      return "Feels like: " + this.info.main.feels_like + "℃"
    },

    showMinTemp() {
      return "Min temperature: " + this.info.main.temp_min + "℃"
    },

    showTempMax() {
      return "Max temperature: " + this.info.main.temp_max + "℃"
    },
  },
  methods: {
    getWeather() {
      if(this.city.trim().length < 2){
        this.error = "Please write more than one character!"
        return false
      }

      this.error = ""

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3d9de74844d28377e81415151cbe6a66`)
        .then(res => (this.info = res.data))
    }
  }
}
</script>

<template>
  <div class="wrapper">
    <h1>Weather App</h1>
    <p>{{ city == "" ? "Find out the weather of the city" : "Weather in " + cityName}}</p>
    <input type="text" v-model="city" placeholder="City">
    <button v-if="city != ''" @click="getWeather()">Get Weather</button>
    <button disabled v-else>Please write the city!</button>
    <p class="error">{{ error }}</p>
    <div class="info" v-if="info != null">
      <div>
        <p class="temp">{{ showTemp }}</p>
        <p class="temp">{{ showFeelsLike }}</p>
      </div>
      <div>
        <p class="temp">{{ showMinTemp }}</p>
        <p class="temp">{{ showTempMax }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .info {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 30px;
  }
  .error {
    color: rgb(117, 13, 13);
  }
  .wrapper { 
    font-family:sans-serif;
    width: 900px;
    height: 500px;
    border-radius: 50px;
    background-color: #1F0F24;
    text-align: center;
    color: #FFF;
  }

  .wrapper h1 {
    padding-top: 50px;
    font-size: 50px;
  }

  .wrapper p {
    font-size: 22px;
    padding-top: 20px;
  }

  .wrapper input {
    text-align: center;
    font-size: 20px;
    width: 200px;
    height: 40px;
    margin-top: 30px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid #110813;
    color: #fcfcfc;
    padding: 5px 8px;
    outline: none;
  }

  .wrapper input:focus {
    border-bottom-color: #6e2d7d;
  }

  .wrapper button {
    background: #e3bc4b;
    color: #FFF;
    border-radius: 10px;
    border: 2px solid #b99935;
    padding: 15px 15px;
    margin-left: 20px;
    font-size: 20px;
    cursor: pointer;
    transition: transform 500ms ease;
  }

  .wrapper button:disabled {
    background: #6b5c2e;
    cursor: not-allowed;
    transition: none;
    transform: none;
  }

  .wrapper button:disabled:hover {
    transform: none;
  }

  .wrapper button:hover {
    transform: scale(1.1) translateY(-5px);
  }
</style>
