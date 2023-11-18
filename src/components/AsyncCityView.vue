<template>
    <div class="flex flex-col flex-1 items-center">
        <!--- Banner -->
        <div v-if="route.query.preview" class="text-white p-4 bg-weather-secondary w-full text-center">
            <p> You are currently previewing this city, click the "+"
            icon to start tracking this city.</p>
        </div>
        <!--- weather overview --->
        <div class="flex flex-col items-center text-white py-12">
            <h1 class="text-4xl mb-2">{{ route.params.city }}</h1>
            <p class="text-sm mb-12">
                {{
                 new Date(weatherData.data.currentTime).toLocaleDateString('en-us', { weekday: 'short', day: '2-digit', month: 'long', })
                }}
                {{
                new Date(weatherData.data.currentTime).toLocaleTimeString(
                    "en-us",
                    {
                    timeStyle: "short",
                    }
                )
                }}
            </p>
            <p class="text-8xl mb-0">
                {{ Math.round(weatherData.data.current.temp) }} &deg
            </p>
           
            <p>
                Feels like 
                {{ Math.round(weatherData.data.current.feels_like) }} &deg
            </p>
            <p class="capitalize">
                {{ weatherData.data.current.weather[0].description }}
            </p>
            <img
            class="w-[150px] h-auto"
            :src="
            `http://openweathermap.org/img/wn/${weatherData.data.current.weather[0].icon}@2x.png`
            "
            alt="Current weather icon"/>

            <hr class="border-white border-opacity-10 border w-full"/>
           <!-- Hourly Weather -->
           <div class="max-w-screen-md w-full py-12">
            <div class="mx-8 tex-white">
                <h2 class="mb-4">Hourly Weather</h2>
            </div>

           </div>
        </div>
    </div>
</template>

<script setup>

import axios from 'axios';
import { useRoute } from 'vue-router';

const route = useRoute();
const getWeatherData = async() => {
    try {
        const weatherData = await axios.get(
      `https://api.openweathermap.org/data/2.5/onecall?lat=${route.query.lat}&lon=${route.query.lng}&exclude={part}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=imperial`
    );
    // cal current date & time
    const localOffset = new Date().getTimezoneOffset() * 60000;
    const utc = weatherData.data.current.dt * 1000 + localOffset;
    weatherData.data.currentTime = utc +1000*weatherData.data.timezone_offset;

    //cal hourly weather offset
    weatherData.data.hourly.forEach((hour) => {
        const utc = hour.dt * 1000 + localOffset;
        hour.currentTime = utc + 1000 * weatherData.data.timezone_offset;
    });
    return weatherData;
    } catch (error) {
        console.log(error);
    }
}

const weatherData = await getWeatherData();
console.log('weatherData', weatherData);
</script>
