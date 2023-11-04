<template>
    <div>

    </div>
</template>

<script setup>

import axios from 'axios';
import { useRoute } from 'vue-router';

const route = useRoute();
const getWeatherData = async() => {
    try {
        const weatherData = await axios.get()
    } catch (error) {
        console.log(error);
    }

}
</script>
