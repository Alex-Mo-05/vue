<template>
    <div id="pagetop">
      <select v-model="list" id="list">
        <option value="809">Shrek 2</option>
        <option value="76600">Avatar: The Way of Water</option>
        <option value="505642">Black Panther: Wakanda Forever</option>
        <option value="502356">The Super Mario Bros. Movie</option>
        <option value="594767">Shazam! Fury of the Gods</option>
        <option value="603692">John Wick: Chapter 4</option>
        <option value="640146">Ant-Man and the Wasp: Quantumania</option>
        <option value="378064">A Silent Voice: The Movie</option>
        <option value="2109">Rush Hour</option>
        <option value="9502">Kung Fu Panda</option>
      </select>
      <button id="getbutton" @click="getmovie()">Get</button>
    </div>
    <div class="container">
      <div id="containertop">
        <div id="left">
          <img id="poster" :src="poster" />
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
        <p id="voteAverage">{{ voteaverage }}</p>
        <p id="voteCount">{{ votecount }}</p>
        <p id="budget">{{ budget }}</p>
      </div>
      <iframe id="trailer" :src="trailer"></iframe>
    </div>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";

const list = ref("809")

let poster = ref("");
let title = ref("");
let tagline = ref("");
let status = ref("");
let language = ref("");
let runtime = ref("");
let popularity = ref("");
let voteaverage = ref("");
let votecount = ref("");
let budget = ref("");
let overview = ref("");
let trailer = ref("");

function getmovie() {
  let movieId = parseInt(document.getElementById("list").value);

  let search = axios.get("https://api.themoviedb.org/3/movie/" + movieId, {
    params: {
      api_key: "5353c690f7c8b76f4af766952589f1b2",
      append_to_response: "videos",
    },
  });

  search.then((movieData) => {

    poster.value = "https://image.tmdb.org/t/p/w500" + movieData.data.poster_path;

    title.value = movieData.data.original_title;
    tagline.value = movieData.data.tagline;
    status.value = `${movieData.data.status}: ${movieData.data.release_date}`;
    language.value = `Languages: ${movieData.data.language}`;
    runtime.value = `Run-time: ${movieData.data.runtime} minutes`;
    popularity.value = `Popularity: ${movieData.data.popularity}`;
    voteAverage.value = `Vote Average: ${movieData.data.vote_average}`;
    voteCount.value = `Vote Count: ${movieData.data.vote_count}`;
    budget.value = `Budget: $${movieData.data.budget}`;
    overview.value = movieData.data.overview;
    trailer.value =
      "https://www.youtube.com/embed/" +
      movieData.data.videos.results.filter((trailer) => trailer.type === "Trailer").at(0)
        .key;
  });
}
</script>

<style scoped>
* {
  font-family: "Poppins", sans-serif;
  padding: 0;
  margin: 5px;
  box-sizing: border-box;
  background-color: rgb(24,23,23);
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
  left: 0px;
}

#poster {
  left: 0px;
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
