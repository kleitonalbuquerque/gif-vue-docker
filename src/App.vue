<template>
  <div id="app">
    <h1>Gif Searcher</h1>
    <input v-model="searchTerm" type="text">
    <sui-button class="button" @click=getGifs()>Search</sui-button>
    <div class="gif-container">
      <img v-for="gif in gifs" :src="gif" :key="gif.id">
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchTerm: "",
      gifs: []
    };
  },
  methods: {
    getGifs() {
      let apiKey = "dc6zaTOxFJmzC";
      let searchEndPoint = "https://api.giphy.com/v1/gifs/search?";
      let limit = 1;
      let url = `${searchEndPoint}&api_key=${apiKey}&q=${
        this.searchTerm
      }&limit=${limit}`;
      fetch(url)
        .then(response => {
          return response.json();
        })
        .then(json => {
          this.buildGifs(json);
        })
        .catch(err => console.log(err));
    },
    buildGifs(json) {
      this.gifs = json.data.map(gif => gif.id).map(gifId => {
        return `https://media.giphy.com/media/${gifId}/giphy.gif`;
      });
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
input {
  padding: 5px;
  margin-bottom: 20px;
}
.button {
  background-color: rgb(0, 172, 0) !important;
  color: white !important;
  padding: 5px 20px;
  border: none;
  display: block;
  margin-left: 5px !important;
  margin: 0 auto;
}
.button:hover {
  background-color: rgb(0, 148, 0) !important;
}
.gif-container {
  margin-top: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>