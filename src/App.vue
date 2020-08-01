<template>
  <div id="app">
    <main>
      <img alt="Riby logo" src="./assets/riby_logo.svg" />
      <h1>Riby Gif App</h1>
      <div class="search-box">
        <input v-model="searchTerm" type="text" class="search-bar" placeholder="Input Gif keyword" />
        <button class="search-button" @click="getGifs()">Search</button>
      </div>

      <div class="gif-container">
        <img v-for="gif in gifs" :src="gif" :key="gif.id" />
      </div>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchTerm: "",
      gifs: [],
    };
  },
  methods: {
    getGifs() {
      let apiKey = "dpXmC7aJSQKeBMaWTd79ScTzkbVACvAU";
      let searchEndPoint = "https://api.giphy.com/v1/gifs/search?";
      let limit = 100;

      let url = `${searchEndPoint}&api_key=${apiKey}&q=${this.searchTerm}&limit=${limit}`;

      fetch(url)
        .then((response) => {
          return response.json();
        })
        .then((json) => {
          this.buildGifs(json);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    buildGifs(json) {
      this.gifs = json.data
        .map((gif) => gif.id)
        .map((gifId) => {
          return `https://media.giphy.com/media/${gifId}/giphy.gif`;
        });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
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

.search-bar {
  width: 50%;
  margin-bottom: 20px;
}

.search-button {
  background-color: rgb(0, 172, 0);
  color: white;
  padding: 5px 20px;
  border: none;
  display: block;
  margin: 0 auto;
}

.search-button:hover {
  background-color: rgb(0, 148, 0);
}

.gif-container {
  margin-top: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
