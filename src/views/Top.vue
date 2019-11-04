<template>
  <div id="top">
    <div v-if="isLoading" class="loading">
      <bounce-loader :color="color" :size="size"></bounce-loader>
    </div>
    <GlobalNav :user="userData"></GlobalNav>
    <Home v-if="!isLogin && !isLoading"></Home>
    <Editor v-if="isLogin && !isLoading" :user="userData"></Editor>
  </div>
</template>

<script>
import Home from "../components/Home.vue";
import Editor from "../components/Editor.vue";
import GlobalNav from "../components/GlobalNav.vue";
import BounceLoader from "vue-spinner/src/BounceLoader.vue";

export default {
  name: "top",
  data() {
    return {
      isLogin: false,
      userData: null,
      isLoading: true,
      color: "#29a972",
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
    GlobalNav: GlobalNav,
    BounceLoader
  }
};
</script>

<style lang="scss" scoped>
.loading {
  position: fixed;
  top: 50%;
  left: 50%;
  margin: -200px 0 0 -200px;
}
</style>