<script setup>
import { ref } from 'vue';
import axios from 'axios';


const list = ref();

const movieImage = ref(null);
const poster = ref(null);
const title = ref(null);
const tagline = ref(null);
const status = ref(null);
const popularity = ref(null);
const voteAverage = ref(null);
const voteCount = ref(null);
const budget = ref(null);
const overview = ref(null);
const trailer = ref(null);


let options = ref([
    {text: "Shrek 2", value: "809"},
    {text: "Avatar: The Way of Water", value: "76600"},
    {text: "Black Panther: Wakanda Forever", value: "505642"},
    {text: "The Super Mario Bros. Movie", value: "502356"},
    {text: "Shazam! Fury of the Gods", value: "594767"},
    {text: "John Wick: Chapter 4", value: "603692"},
    {text: "Ant-Man and the Wasp: Quantumania", value: "640146"},
    {text: "A Silent Voice: The Movie", value: "378064"},
    {text: "Rush Hour", value: "2109"},
    {text: "Kung Fu Panda", value: "9502"},
]);

function getMovieData() {
  let search = axios.get(`https://api.themoviedb.org/3/movie/${dropdownMenu.value}`, {
    params: {
      api_key: "5353c690f7c8b76f4af766952589f1b2",
      append_to_response: "videos",
    }
  })

  let searchResult = search.then((movieData) => {
    movieImage.value.style.backgroundImage = `url(https://image.tmdb.org/t/p/w500/${movieData.data.backdrop_path})`;
    poster.value = "https://image.tmdb.org/t/p/w500" + movieData.data.poster_path;
    title.value = movieData.data.original_title;
    tagline.value = movieData.data.tagline;
    status.value = `${movieData.data.status} ~ ${movieData.data.release_date}`;
    popularity.value = `Popularity: ${movieData.data.popularity}`;
    voteAverage.value = `Vote Average: ${movieData.data.vote_average}`;
    voteCount.value = `Vote Count: ${movieData.data.vote_count}`;
    budget.value = `Budget: $${movieData.data.budget}`;
    overview.value = movieData.data.overview;
    trailer.value = "https://www.youtube.com/embed/" + (movieData.data.videos.results.filter((trailer) => trailer.type === "Trailer")).at(0).key;
  })
}

</script>

<template>
  <head>
    <title>TMDB API</title>
    <div id="pagetop">
      <select v-model="dropdownMenu" id="list" name="Select">
        <option v-for="option in options" :value=option.value>{{ option.text }}</option>
      </select>
      <button id="getbutton" @click="getMovieData">Get</button>
    </div>
  </head>

  <body>
    <div class="container">
      <div id="containertop">
        <div id="left">
          <img id="poster" :src="poster">
        </div>
        <div id="right">
          <h1 id="title">{{ title }}</h1>
          <h3 id="tagline">{{ tagline }}</h3>
          <p id="overview">{{ overview }}</p>
        </div>
      </div>
      <div id="containerbot">
        <h3 id="status">{{ status }}</h3>
        <p id="language">{{ language }}</p>
        <p id="runtime">{{ runtime }}</p>
        <p id="popularity">{{ popularity }}</p>
        <p id="voteAverage">{{ voteAverage }}</p>
        <p id="voteCount">{{ voteCount }}</p>
        <p id="budget">{{ budget }}</p>
      </div>
      <iframe id="trailer" :src="trailer"></iframe>
    </div>
  </body>
</template>

<style scoped>
* {
  font-family: "Poppins", sans-serif;
  padding: 0;
  margin: 5px;
  box-sizing: border-box;
  background-color: rgb(24, 23, 23);
}

.container {
  margin-top: 20px;
  width: 100%;
  align-items: center;
}

#pagetop {
  display: flex;
  margin-left: auto;
  margin-right: auto;
  margin-top: 20px;
  text-align: center;
  width: 20%;
  height: 40px;
}

#list {
  color: azure;
  border-radius: 5px;
}

#getbutton {
  margin-left: 5px;
  color: azure;
  border-radius: 5px;
}

#containertop {
  height: 100vh;
  width: 100%;
  justify-content: space-between;
  display: flex;
}

.left {
  width: 40%;
}

img {
  border-radius: 8px;
}

.right {
  width: 55%;
}

h1 {
  color: rgb(225, 218, 218);
  font-size: 60px;
}

h3 {
  color: rgb(175, 169, 169);
  font-size: 35px;
}

p {
  color: rgb(158, 156, 156);
  font-size: 20px;
}

#containerbot {
  overflow: hidden;
  width: 100%;
}

iframe {
  bottom: 0px;
  float: right;
  border-radius: 8px;
  position: sticky;
  aspect-ratio: 16/9;
  width: 50%;
}
</style>
