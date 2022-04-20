<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      albums: [],
    };
  },
  created: function () {
    this.albumsIndex();
    this.newsIndex();
    var code = this.$route.query.code;
    if (code) {
      axios.get("/auth/github/callback?code=" + code).then((response) => {
        localStorage.setItem("github_access_token", response.data.access_token);
        this.$router.push("/about");
      });
    }
    console.log(this.$route.query.code);
  },
  methods: {
    albumsIndex: function () {
      axios.get("https://jsonplaceholder.typicode.com/albums").then((response) => (this.albums = response.data));
    },
    newsIndex: function () {
      axios.get("/news_api").then((response) => {
        console.log(response.data);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <a href="https://github.com/login/oauth/authorize?client_id=a87231a022a1a5793102">Sign into GitHub</a>
    <div v-for="album in albums" v-bind:key="album.id">
      <h2>{{ album.title }}</h2>
    </div>
  </div>
</template>

<style></style>
