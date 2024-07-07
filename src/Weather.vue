<script setup>
import WeatherInfo from './WeatherInfo.vue';
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
                info.value = res.data.main
                weather.value = res.data.weather[0].main
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
    <h1>Weather app</h1>
    <div class="info-block" v-bind:style="{
        backgroundImage: `url(/src/img/weather-img/${weather}.png)`,
        backgroundSize: 'cover',
        backgroundRepeat: 'no-repeat',
        backgroundPosition: '40px 75px',
    }">
        <div class="welcome-string">Find out the weather in {{ city === '' ? 'your' : cityName }} city</div>
        <div>
            <input type="text" v-model="city" placeholder="Enter the name of the city" />
            <button @click="getWeather">Find out the weather</button>
        </div>
        <p v-show="error !== ''" className="error">{{ error }}</p>
        <div v-show="error === '' && isLoading">Loading...</div>

        <WeatherInfo :info="info" :isLoading="isLoading" :error="error" :weather="weather" />
    </div>
</template>

<style scoped>
h1 {
    display: flex;
    justify-content: center;
    width: 100%;
    color: #1f0f24;
    font-family: 'Playwrite IT Moderna', cursive;
    font-optical-sizing: auto;
}

.info-block {
    text-shadow: 1px 1px 2px black;
    height: 450px;
    display: flex;
    flex-direction: column;
    align-items: center;
    font-family: 'Josefin Sans', sans-serif;
    font-optical-sizing: auto;
    font-weight: 700;
    font-style: normal;
    border-radius: 50px;
    padding: 30px;
    background: #110813;
    text-align: center;
}

@media (min-width: 1280px) {
    .info-block {
        width: 50%;
    }
}

@media (min-width: 720px) and (max-width: 1280px) {
    .info-block {
        width: 70%;
    }
}

@media (max-width: 720px) {
    .info-block {
        width: 80%;
    }
}

.welcome-string {
    margin-top: 15px;
}

.info-block p {
    /* -webkit-text-stroke: 1px rgba(0, 0, 0, 0.281); */
    text-shadow: 1px 1px 2px black;
    margin-top: 0;
}

.info-block input {
    margin-top: 30px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid #110813;
    color: white;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
}

input::placeholder {
    color: rgb(255, 255, 255);
    width: 125%;
}

.info-block input:focus {
    border-bottom-color: #6e2d7d;
}

.info-block button {
    margin-top: 20px;
    background: #e3bc4b;
    border: 2px solid #b99935;
    border-radius: 10px;
    color: white;
    padding: 10px 15px;
    cursor: pointer;
    transition: transform 500ms ease;
    margin-bottom: 10px;
}

.info-block button:hover {
    transform: scale(1.1) translateY(-5px);
}

.info-block button:active {
    background: #f3d36f;
}

.error {
    color: red;
}
</style>
