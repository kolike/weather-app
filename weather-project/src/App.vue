<script>
import axios from 'axios'

export default {
    data() {
        return {
            city: '',
            error: '',
            info: null,
            loader: 'Loading...',
            firstLoading: true,
            cityValue: '',
        }
    },
    computed: {
        cityName() {
            return "'" + this.city + "'"
        },
        showTemp() {
            return 'Air temperature: ' + Math.round(this.info.temp) + '°C'
        },
        showFeelsLike() {
            return 'Feels like: ' + Math.round(this.info.feels_like) + '°C'
        },
        swhorMinTemp() {
            return 'Min temperature: ' + Math.round(this.info.temp_min) + '°C'
        },
        showMaxTemp() {
            return 'Max temperature: ' + Math.round(this.info.temp_max) + '°C'
        },
        showHumidity() {
            return 'Humidity: ' + this.info.humidity + '%'
        },
    },
    methods: {
        getWeather() {
            if (this.city.trim().length < 2) {
                this.error = 'Invalid city name'
                return false
            }
            if (this.cityValue !== this.city) {
                axios
                    .get(
                        `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=fd0c066f40c0aa2abd192e5b2fbbd420`
                    )
                    .then((res) => (this.info = res.data.main))
                    .catch(() => (this.error = 'Incorrect city name'))
                this.cityValue = this.city
            }
            this.firstLoading = false
            this.error = ''
        },
    },
}
</script>

<template>
    <h1>Weather app</h1>
    <div className="wrapper">
        <p>Find out the weather in {{ city === '' ? 'your' : cityName }} city</p>
        <input type="text" v-model="city" placeholder="Enter the name of the city" />
        <button v-show="city != ''" @click="getWeather()">Find out the weather</button>
        <p className="error">{{ error }}</p>
        <div v-show="info == null && error == '' && firstLoading == false">{{ loader }}</div>
        <div className="showInfo" v-if="info !== null && error == ''">
            <p>{{ showTemp }}</p>
            <p>{{ showHumidity }}</p>
            <p>{{ showFeelsLike }}</p>
            <p>{{ swhorMinTemp }}</p>
            <p>{{ showMaxTemp }}</p>
        </div>
    </div>
</template>

<style scoped>
.error {
    color: red;
}
.wrapper {
    width: 90%;
    font-family: 'Josefin Sans', sans-serif;
    font-optical-sizing: auto;
    font-weight: 300;
    font-style: normal;
    border-radius: 50px;
    padding: 30px;
    background: #1f0f24;
    text-align: center;
    color: white;
    display: flex;
    flex-direction: column;
    align-items: center;
    background: linear-gradient(339deg, rgb(0, 69, 219) 0%, rgb(255, 101, 40) 100%);
}
h1 {
    font-family: 'Playwrite IT Moderna', cursive;
    font-optical-sizing: auto;
    font-weight: 500;
    font-style: normal;
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
    color: white;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
}
input::placeholder {
    font-family: 'Playwrite IT Moderna', cursive;
    font-optical-sizing: auto;
    font-weight: 500;
    font-style: normal;
    width: 125%;
}

.wrapper input:focus {
    border-bottom-color: #6e2d7d;
}

.wrapper button {
    margin-top: 20px;
    background: #e3bc4b;
    border: 2px solid #b99935;
    border-radius: 10px;
    color: white;
    padding: 10px 15px;
    cursor: pointer;
    transition: transform 500ms ease;
}

.wrapper button:hover {
    transform: scale(1.1) translateY(-5px);
}
.wrapper button:active {
    background: #c0a03f;
}

.showInfo {
    display: flex;
    flex-direction: column;
    align-items: start;
}

/* @media (min-width: 300px) and (max-width: 500px) {
    .wrapper {
        width: 100%;
    }
} */
</style>
