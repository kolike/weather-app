<script setup>
import { computed } from 'vue';
const props = defineProps({
    city: String,
    info: String || Object,
    isLoading: Boolean,
    error: String,
    weather: String
})


const showTemp = computed(() => {
    return 'Air temperature: ' + Math.round(props.info.temp) + '°C'
})
const showFeelsLike = computed(() => {
    return 'Feels like: ' + Math.round(props.info.feels_like) + '°C'
})
const showMinTemp = computed(() => {
    return 'Min temperature: ' + Math.round(props.info.temp_min) + '°C'
})
const showMaxTemp = computed(() => {
    return 'Max temperature: ' + Math.round(props.info.temp_max) + '°C'
})
const showHumidity = computed(() => {
    return 'Humidity: ' + props.info.humidity + '%'
})

</script>
<template>
    <div className="showInfo" v-if="info !== null && error === '' && !isLoading">
        <p>{{ weather }}</p>
        <p>{{ showTemp }}</p>
        <p>{{ showHumidity }}</p>
        <p>{{ showFeelsLike }}</p>
        <p>{{ showMinTemp }}</p>
        <p>{{ showMaxTemp }}</p>
    </div>
</template>
<style>
.showInfo {
    display: flex;
    flex-direction: column;
    align-items: start;
}
</style>
