<template>
  
  <h1 class="flex justify-center text-center text-6xl lg:text-8xl font-black text-yellow-500 p-2 md:p-5">STAR WARS APP</h1>
  <div v-if="films" class="p-8 flex items-center justify-center h-full">
    <ul class="grid sm:grid-cols-3 gap-10">
      <li v-for="movie in films" :key="getId(movie.url)" class="flex items-center gap-3">
        <div class="text-yellow-500 bg-transparent shadowBox rounded-xl w-[500px] lg:h-60 sm:h-80 p-8 hover:shadow-[0px_0px_40px_8px_rgba(253,224,0,0.4)] hover:scale-105">
          <h3 class="font-bold text-3xl lg:text-4xl mb-1">{{ movie.title }}</h3>
          <p class="text-yellow-100 text-lg lg:text-xl">{{ movie.director }}</p>
          <p class="text-yellow-100 mb-5 text-lg lg:text-xl">{{ movie.release_date }}</p>
          <RouterLink :to="`/film/${getId(movie.url)}`" class="border-yellow-500 border font-semibold text-yellow-500 rounded-lg p-2 hover:text-black hover:bg-yellow-500">See Details</RouterLink>
        </div>
      </li>
    </ul>
 
    <!-- LOADING -->
  <div v-if="loading">
    <div class="flex flex-row gap-2 justify-center items-center">
      <svg version="1.2" baseProfile="tiny" id="Ebene_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="60px" height="60px" viewBox="0 0 42 42" xml:space="preserve" class="icon animate-bounce [animation-delay:-.7s]"> <path d="M36,17.4V12c0-6.6-5.4-12-12-12h-6C11.399,0,6,5.4,6,12v5.4L0,33c0,3,9.401,9,21,9c11.598,0,21-6,21-9L36,17.4z M12,37.125 c-0.622,0-1.125-0.503-1.125-1.125s0.503-1.125,1.125-1.125s1.125,0.503,1.125,1.125S12.622,37.125,12,37.125 M10.5,21 c0-0.825,0.416-2.031,0.925-2.68c0,0,1.426-1.82,4.325-1.82c1.471,0,2.497,0.676,2.497,0.676c0.688,0.453,1.253,1.499,1.253,2.324 v2.25c0,0.413-0.675,0.75-1.5,0.75h-6C11.175,22.5,10.5,21.825,10.5,21 M16.5,36H15v-3l1.5-1.5V36z M20.25,36H18v-6h2.25V36z M18.75,28.5v-2.25c0-1.243,1.008-2.25,2.25-2.25c1.243,0,2.25,1.007,2.25,2.25v2.25H18.75z M24,36h-2.25v-6H24V36z M27,36h-1.5 v-4.5L27,33V36z M30,37.125c-0.622,0-1.125-0.503-1.125-1.125s0.503-1.125,1.125-1.125s1.125,0.503,1.125,1.125 S30.622,37.125,30,37.125 M30,22.5h-6c-0.825,0-1.5-0.337-1.5-0.75V19.5c0-0.825,0.563-1.871,1.253-2.324 c0,0,1.025-0.676,2.497-0.676c2.899,0,4.325,1.82,4.325,1.82C31.084,18.969,31.5,20.175,31.5,21S30.825,22.5,30,22.5 M39.271,33.699 C39.183,33.733,39.091,33.75,39,33.75c-0.3,0-0.584-0.182-0.698-0.478l-5.455-14.012c-1.442-3.604-3.225-5.011-6.357-5.011 c-2.079,0-3.822,1.289-3.84,1.303c-0.925,0.694-2.374,0.691-3.3,0c-0.015-0.014-1.775-1.303-3.839-1.303 c-3.132,0-4.916,1.407-6.36,5.017L3.699,33.272c-0.15,0.386-0.588,0.577-0.972,0.427c-0.386-0.149-0.577-0.584-0.426-0.972 l5.454-14.011c1.673-4.184,3.992-5.967,7.755-5.967c2.574,0,4.652,1.537,4.739,1.601c0.393,0.296,1.107,0.296,1.499,0 c0.088-0.064,2.167-1.601,4.74-1.601c3.763,0,6.081,1.783,7.753,5.961l5.457,14.017C39.849,33.115,39.658,33.55,39.271,33.699"/>
      </svg>
    </div>    
  </div>

 </div>
</template>

<script>
import axios from "axios";
import { RouterLink } from 'vue-router';

export default {
  name: "Home",
  data() {
    return {
      films: [],
      loading: false,
      error: "",
    };
  },

  created(){
    this.getFilms()
  },

  methods: {
    getFilms() {
      this.loading = true;
      axios.get('https://swapi.dev/api/films')
        .then(response => {
          if (response.data.results) {
            this.films = response.data.results
            this.loading = false
          } else {
            this.error = 'No data found, Sorry!'
            this.loading = false
          }
        })
        .catch(error => {
          this.error = error.message
          this.loading = false
        })
    },
    getId(url) {
      let urlId =  url.split('/').reverse()[1];
      return urlId
    },
  },
};
</script>

<style>
.shadowBox {
  box-shadow: inset 0px 0px 20px rgba(61, 61, 61, 0.5);
}
.icon {
  fill: rgb(234, 179, 8);
}
</style>