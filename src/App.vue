<script>
import axios from 'axios'

export default {
  data() {
    return {
      city: '',
      error: '',
      info: null
    }
  },
  computed: {
    showTemp() {
      return 'Temperature: ' + this.info.main.temp
    },
    showFeelsLike() {
      return 'Feels like: ' + this.info.main.feels_like
    }
  },
  methods: {
    getWeather() {
      if(this.city.trim().length < 2) {
          this.error = 'Length must be greater then 1 character'
          return false;
      }

      this.error = ''

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=a3c4240f8b79cdbc62fd5a71c0b7eb4e`)
      .then(res => (this.info = res.data))
    }
  }
}
</script>

<template>
<div class="wrapper">
  <h1>Weather forecast</h1>
  <p>find out the weather in {{ city == '' ? 'your city' : "«" + city + "»"}}</p>
  <input v-model="city" type="text" placeholder="Enter your city" />
  <button v-if="city != ''" @click="getWeather()">Get a weather</button>
  <button disabled v-else>Enter your city</button>
  <p class="error">{{ error }}</p>

  <P>{{ city == '' ? info = null : ''}}</P>
  <div v-if="info != null">
    <p>{{ showTemp }}</p>
    <p>{{ showFeelsLike }}</p>
  </div>
</div>
</template>

<style scoped>
.error {
  color: rgb(167, 19, 19);
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: #1f0f24;
  text-align: center;
  color: #fff;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
   border-bottom-color: #6e2d7d;
}

.wrapper button {
  background: #e3bc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 400ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}

.wrapper button:disabled {
  background: #776738;
  transform: scale(1.0);
  cursor: not-allowed;
}
</style>
