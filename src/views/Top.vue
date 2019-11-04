<template>
  <div id="top">
    <div v-if="isLoading" class="loading">
      <bounce-loader :color="color" :size="size"></bounce-loader>
    </div>
    <Home v-if="!isLogin && !isLoading"></Home>
    <Editor v-if="isLogin && !isLoading" :user="userData"></Editor>
    <router-link :to="{ name: 'terms' }">利用規約</router-link>
  </div>
</template>

<script>
import Home from "../components/Home.vue";
import Editor from "../components/Editor.vue";
import BounceLoader from "vue-spinner/src/BounceLoader.vue";

export default {
  name: "top",
  data() {
    return {
      isLogin: false,
      userData: null,
      isLoading: true,
      color: "#5dc596",
      size: "400px"
    };
  },
  created: function() {
    firebase.auth().onAuthStateChanged(user => {
      console.log(user);
      if (user) {
        this.isLogin = true;
        this.userData = user;
      } else {
        this.isLogin = false;
        this.userData = null;
      }
      this.isLoading = false;
    });
  },
  components: {
    Home: Home,
    Editor: Editor,
    BounceLoader
  }
};
</script>

<style lang="scss">
#top {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
a {
  color: #42b983;
}
.loading {
  position: fixed;
  top: 50%;
  left: 50%;
  margin: -200px 0 0 -200px;
}
</style>