<script setup>
import { computed } from 'vue'
const props = defineProps({
    city: String,
    info: [String, Object],
    isLoading: Boolean,
    error: String,
    weather: String,
})

const showTemp = computed(() => {
    return Math.round(props.info.main.temp) + '°C'
})
const showFeelsLike = computed(() => {
    return 'Feels like: ' + Math.round(props.info.main.feels_like) + '°C'
})
const showMinTemp = computed(() => {
    return 'Min temperature: ' + Math.round(props.info.main.temp_min) + '°C'
})
const showMaxTemp = computed(() => {
    return 'Max temperature: ' + Math.round(props.info.main.temp_max) + '°C'
})
const showHumidity = computed(() => {
    return props.info.main.humidity + '%'
})
const showPressure = computed(() => {
    return props.info.main.pressure + ' mm Hg'
})
const showWind = computed(() => {
    return props.info.wind.speed + ' m/sec'
})

const weatherImgSrc = computed(() => {
    return `../src/img/${props.weather.toLowerCase()}.png`
})
</script>
<template>
    <div className="content-info" v-if="info !== null && error === '' && !isLoading">
        <div class="temperature">
            <p class="temperature-now">{{ showTemp }}</p>
            <img class="weather-img-src" :src="weatherImgSrc" />
            <div class="weather-info">
                <p class="weather-text">{{ weather }}</p>
                <p>{{ showFeelsLike }}</p>
            </div>
        </div>
        <div class="other-weather-info">
            <p class="other"><img src="../src/img/wind.png" /> {{ showWind }}</p>
            <p class="other"><img src="../src/img/humidity.png" /> {{ showHumidity }}</p>
            <p class="other"><img src="../src/img/pressure.png" /> {{ showPressure }}</p>
        </div>
        <div class="max-temp">{{ showMaxTemp }}</div>
        <div class="min-temp">{{ showMinTemp }}</div>
    </div>
</template>
<style>
.content-info {
    display: flex;
    flex-direction: column;
    align-items: stretch;
}
.temperature {
    margin-top: 30px;
    display: flex;
    flex-direction: row;
    align-items: center;
}
.temperature-now {
    margin: 0px;
    padding-top: 10px;
    font-size: 70px;
}
.weather-text::first-letter {
    text-transform: uppercase;
}
.weather-img-src {
    width: 100px;
    height: 100px;
}
p img {
    width: 20px;
    height: 20px;
}
.weather-info {
    text-align: left;
}

.other-weather-info {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    margin: 0;
}
.max-temp {
    text-align: left;
    padding: 10px 0px;
}
.min-temp {
    text-align: left;
    padding: 10px 0px;
}
.other {
    display: flex;
    align-items: flex-end;
}
.other img {
    margin-right: 5px;
}
</style>
