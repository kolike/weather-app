<script>
import axios from 'axios'

export default {
    data() {
        return {
            city: '',
            error: '',
            info: null,
            isLoading: false,
            prevCity: '',
            weather: '',
        }
    },
    computed: {
        cityName() {
            return "'" + this.city + "'"
        },
        showTemp() {
            return 'Air temperature: ' + Math.round(this.info.main.temp) + '°C'
        },
        showFeelsLike() {
            return 'Feels like: ' + Math.round(this.info.main.feels_like) + '°C'
        },
        showMinTemp() {
            return 'Min temperature: ' + Math.round(this.info.main.temp_min) + '°C'
        },
        showMaxTemp() {
            return 'Max temperature: ' + Math.round(this.info.main.temp_max) + '°C'
        },
        showHumidity() {
            return 'Humidity: ' + this.info.main.humidity + '%'
        },
    },
    methods: {
        getWeather() {
            if (this.city.trim().length < 2) {
                this.error = 'Invalid city name'
                return false
            }
            if (this.prevCity !== this.city) {
                this.isLoading = true
                axios
                    .get(
                        `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=fd0c066f40c0aa2abd192e5b2fbbd420`
                    )
                    .then((res) => {
                        this.info = res.data
                        this.weather = this.info.weather[0].main
                    })
                    .catch(() => {
                        this.error = 'Incorrect city name'
                    })
                    .finally(() => (this.isLoading = false))
                this.prevCity = this.city
            }

            this.error = ''
        },
    },
}
</script>

<template>
    <h1>Weather app</h1>
    <div
        class="info-block"
        :style="{
            backgroundImage: `url(/src/img/weather-img/${weather}.png)`,
            backgroundSize: 'cover',
            backgroundRepeat: 'no-repeat',
            backgroundPosition: '40px 75px',
        }"
    >
        <div class="welcome-string">Find out the weather in {{ city === '' ? 'your' : cityName }} city</div>
        <input type="text" v-model="city" placeholder="Enter the name of the city" />
        <button @click="getWeather()">Find out the weather</button>
        <p v-show="error !== ''" className="error">{{ error }}</p>
        <div v-show="error === '' && isLoading">Loading...</div>
        <div className="showInfo" v-if="info !== null && error === '' && !isLoading">
            <p><img src="/src/img/icons/weather.png" /> {{ weather }}</p>
            <p><img src="/src/img/icons/celsius.png" />{{ showTemp }}</p>
            <p><img src="/src/img/icons/humidity.png" />{{ showHumidity }}</p>
            <p><img src="/src/img/icons/celsius.png" />{{ showFeelsLike }}</p>
            <p><img src="/src/img/icons/cold.png" />{{ showMinTemp }}</p>
            <p><img src="/src/img/icons/hot.png" />{{ showMaxTemp }}</p>
            <!-- MVC -->
        </div>
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
        width: 20%;
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

p img {
    width: 30px;
    height: 30px;
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

.showInfo {
    display: flex;
    flex-direction: column;
    align-items: start;
}

.error {
    color: red;
}
</style>

<!-- { "coord": { "lon": 41.3192, "lat": 56.3572 }, "weather": [ { "id": 800, "main": "Clear", "description": "clear sky",
"icon": "01d" } ], "base": "stations", "main": { "temp": 29.08, "feels_like": 29.64, "temp_min": 29.08,
"temp_max":29.08, "pressure": 1006, "humidity": 49, "sea_level": 1006, "grnd_level": 993 }, "visibility": 10000, "wind":
{ "speed":3.74, "deg": 197, "gust": 5.53 }, "clouds": { "all": 0 }, "dt": 1719991289, "sys": { "country": "RU",
"sunrise":1719966740, "sunset": 1720029919 }, "timezone": 10800, "id": 543460, "name": "Kovrov", "cod": 200 } {
"coord":{ "lon": 30.2642, "lat": 59.8944 }, "weather": [ { "id": 520, "main": "Rain", "description": "light intensity
shower rain", "icon": "09d" } ], "base": "stations", "main": { "temp": 15.59, "feels_like": 15.58, "temp_min": 15.59,
"temp_max": 16.08, "pressure": 999, "humidity": 91, "sea_level": 999, "grnd_level": 997 }, "visibility": 10000, "wind":{
"speed": 2, "deg": 310 }, "rain": { "1h": 1.15 }, "clouds": { "all": 75 }, "dt": 1719991523, "sys": { "type": 2,
"id":197864, "country": "RU", "sunrise": 1719967490, "sunset": 1720034476 }, "timezone": 10800, "id": 498817, "name":
"Saint Petersburg", "cod": 200 } -->
