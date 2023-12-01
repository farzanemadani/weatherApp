<template>
    <div class="flex p-3 bg-weather-secondary rounded-md shadow-md">
        <div class="flex flex-col flex-1 gap-2">
            <AnimatedPlaceholder class="max-w-[50%] rounded-md"/>
            <AnimatedPlaceholder class="max-w-[40%] rounded-md"/>

        </div>

        <div class="flex flex-col items-end flex-1 gap-2">
            <AnimatedPlaceholder class="max-w-[50px] w-full rounded-md"/>
            <AnimatedPlaceholder class="max-w-[75px] w-full rounded-md"/>
        </div>
    </div>
</template>

<script setup>
import AnimatedPlaceholder from './AnimatedPlaceholder.vue';

</script>
