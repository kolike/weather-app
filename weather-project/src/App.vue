<script>
import axios from 'axios'

export default {
    data() {
        return {
            city: '',
            error: '',
            info: null,
        }
    },
    computed: {
        cityName() {
            return "'" + this.city + "'"
        },
        showTemp() {
            return 'Air temperature: ' + this.info.temp
        },
        showFeelsLike() {
            return 'Feels like: ' + this.info.feels_like
        },
        swhorMinTemp() {
            return 'Min temperature: ' + this.info.temp_min
        },
        showMaxTemp() {
            return 'Max temperature: ' + this.info.temp_max
        },
    },
    methods: {
        async getWeather() {
            if (this.city.trim().length < 2) {
                this.error = 'Invalid city name'
                return false
            }
            this.error = ''

            axios
                .get(
                    `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=fd0c066f40c0aa2abd192e5b2fbbd420`
                )
                .then((res) => (this.info = res.data.main))
        },
    },
}
</script>

<template>
    <div className="wrapper">
        <h1>Weather app</h1>
        <p>Find out the weather in {{ city === '' ? 'your' : cityName }} city</p>
        <input type="text" v-model="city" placeholder="Enter the name of the city" />
        <button v-show="city != ''" @click="getWeather()">Find out the weather</button>
        <p className="error">{{ error }}</p>
        <div v-if="info !== null">
            <p>{{ showTemp }}</p>
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
    width: 900px;
    height: 500px;
    border-radius: 50px;
    padding: 20px;
    background: #1f0f24;
    text-align: center;
    color: white;
}
.wrapper h1 {
    margin-top: 50px;
}
.wrapper p {
    margin-top: 50px;
}
.wrapper input {
    margin-top: 30px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid #110813;
    color: silver;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
}

.wrapper input:focus {
    border-bottom-color: #6e2d7d;
}

.wrapper button {
    margin-left: 20px;
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
</style>
