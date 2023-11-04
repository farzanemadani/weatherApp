<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input type="text" 
      @input="getSearchResults"
      v-model="searchQuery"
      placeholder="Search for acity or state" 
      class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"/>
      <ul class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]">
        <p v-if="searchError">
          No results match your query, try a different term.
        </p>
        <p v-if="!searchError && mapBoxResultsCount === 0">
          No results match your query, try a different term.
        </p>
        <template v-else>
          <li
          v-for="searchResult in mapBoxResults"
          :key="searchResult.id"
          class="py-2 cursor-pointer">
            {{ searchResult.place_name }}
          </li>
        </template>
      </ul>
    </div>
    
  
  </main>
</template>
<script setup>
import { ref } from 'vue'; 
import { unref } from 'vue'
import axios from 'axios';

const searchQuery = ref('');
const queryTimeout = ref(null);
const mapboxApiKey = "pk.eyJ1Ijoiam9obmtvbWFybmlja2kiLCJhIjoiY2t5NjFzODZvMHJkaDJ1bWx6OGVieGxreSJ9.IpojdT3U3NENknF6_WhR2Q";
const mapBoxResults = ref(null);
const mapBoxResultsCount = ref(0);

const searchError = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async() => {
    if(searchQuery.value !== ''){
      try{
          const result = await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxApiKey}&types=place`);
          mapBoxResults.value = result.data.features;
          console.log('mapBoxResults', mapBoxResults);
          mapBoxResultsCount.value = unref(mapBoxResults.value).length;
      }catch{
        searchError.value = true
      }
      return;
    }
    mapBoxResults.value = null;
  } , 300);

}

</script>


