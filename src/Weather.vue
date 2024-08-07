<script setup>
import WeatherInfo from './WeatherInfo.vue'
import { ref, computed } from 'vue'
import axios from 'axios'

const city = ref('')
const error = ref('')
const info = ref('')
const isLoading = ref('')
const prevCity = ref('')
const weather = ref('')

function getWeather() {
    if (city.value.trim().length < 2) {
        error.value = 'Invalid city name'
        return false
    }
    if (prevCity.value !== city.value) {
        isLoading.value = true
        axios
            .get(
                `https://api.openweathermap.org/data/2.5/weather?q=${city.value}&units=metric&appid=fd0c066f40c0aa2abd192e5b2fbbd420`
            )
            .then((res) => {
                info.value = res.data
                weather.value = res.data.weather[0].main.toLowerCase()
            })
            .catch((res) => {
                error.value = res.response.data.message
            })
            .finally(() => (isLoading.value = false))
        prevCity.value = city.value
    }
    error.value = ''
}

const cityName = computed(() => {
    return "'" + city.value + "'"
})
</script>

<template>
    <div class="info-block" :class="weather">
        <div class="welcome-string">Find out the weather in {{ city === '' ? 'your' : cityName }} city</div>

        <div class="search-panel">
            <input @keyup.enter="getWeather" type="text" v-model="city" placeholder="Enter the name of the city" />
            <button @click="getWeather">Search</button>
        </div>
        <p v-show="error !== ''" class="error">{{ error }}</p>
        <div class="loading" v-show="error === '' && isLoading">Loading...</div>
        <WeatherInfo v-if="info !== '' && error === '' && !isLoading" :info="info" :weather="weather" />
    </div>
</template>

<style scoped>
.info-block {
    text-shadow: 1px 1px 2px black;
    height: 350px;
    display: flex;
    flex-direction: column;
    align-items: center;
    font-family: 'LXGW WenKai TC', cursive;
    font-optical-sizing: auto;
    font-weight: 700;
    font-style: normal;
    border-radius: 50px;
    padding: 30px;
    background: linear-gradient(339deg, rgba(0, 90, 97, 0.568) 0%, rgba(0, 144, 155, 0.527) 100%);
    text-align: center;
}

.search-panel {
    display: flex;
    align-items: center;
    margin-top: 15px;
}

.clear {
    background: linear-gradient(339deg, rgb(134, 179, 247) 0%, rgb(173, 239, 255) 100%);
}

.clouds,
.rain,
.drizzle {
    background: linear-gradient(339deg, rgb(248, 248, 248) 0%, rgb(206, 206, 206) 100%);
    color: black;
    text-shadow: none;
}

.snow,
.mist {
    background: linear-gradient(339deg, rgb(255, 255, 255) 0%, rgba(207, 206, 206, 0.521) 100%);
    color: black;
    text-shadow: none;
}

.thunderstorm {
    background: linear-gradient(339deg, rgb(202, 202, 202) 0%, rgba(75, 75, 75, 0.521) 100%);
    color: black;
    text-shadow: none;
}

@media (min-width: 1280px) {
    .info-block {
        width: 40%;
    }
}

@media (min-width: 720px) and (max-width: 1280px) {
    .info-block {
        width: 60%;
        height: 110%;
    }
}

@media (max-width: 720px) {
    .info-block {
        width: 80%;
        height: 120%;
    }
}
@media (max-width: 720px) and (orientation: landscape) {
    .info-block {
        scale: 60%;
        margin-top: -80px;
    }
}

@media (min-width: 720px) and (max-width: 1280px) and (orientation: landscape) {
    .info-block {
        scale: 65%;
        margin-top: -80px;
    }
}

.welcome-string {
    margin-top: 15px;
    height: 20px;
}

.info-block p {
    text-shadow: 1px 1px 2px black;
    margin-top: 0;
}

.info-block input {
    width: 200px;
    background: #fff url('/src/assets/img/search.png') 10px/8% no-repeat;
    font-family: 'LXGW WenKai TC', cursive;
    margin: 5px;
    border-radius: 15px;
    padding: 10px 0px 10px 35px;
    outline: none;
}

.info-block button {
    background: #4bb3e3;
    padding: 10px 15px;
    border: 2px solid #00000069;
    border-radius: 15px;
    color: white;
    cursor: pointer;
    transition: transform 500ms ease;
    margin: 5px;
}

.info-block button:hover {
    transform: scale(1.1) translateY(-5px);
}

.info-block button:active {
    background: #8bd8fc;
}

.loading {
    margin-top: 15px;
}

.error {
    padding-top: 15px;
    color: red;
}
</style>
