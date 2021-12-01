<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <a href="https://github.com/login/oauth/authorize?client_id=e1d0653dfbe54d01b15f">Sign into GitHub</a>

     <div v-for="post in posts">
      <h2>{{ post.title }}</h2>
      <p>{{ post.body }}</p>
    </div>

  </div>
</template>

<style></style>

<script>
import axios from 'axios'
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      posts: []
    };
  },
  created: function () {
    axios.get("https://jsonplaceholder.typicode.com/posts")
    .then(response => {
      console.log("jsonplaceholder", response.data);
      this.posts = response.data;
    });

    axios
      .get("/news_headlines")
      .then(response => {
        console.log("news api", response.data);
      });

    var code = this.$route.query.code;
    if (code) {
      axios.get("/auth/github/callback?code=" + code).then((response) => {
        localStorage.setItem("github_access_token", response.data.access_token);
        this.$router.push("/about");
      });
    }



  },
  methods: {},
};
</script>
