<template>
  <div id="home">
    <h1>Welcome to Til Note!</h1>
    <button class="login btn-square" @click="googleLogin">Googleアカウントでログイン</button>
    <footer>
      <router-link :to="{ name: 'terms' }">利用規約</router-link>
    </footer>
  </div>
</template>

<script>
export default {
  name: "home",
  data() {
    return {};
  },
  methods: {
    googleLogin: function() {
      firebase
        .auth()
        .signInWithRedirect(new firebase.auth.GoogleAuthProvider());
    }
  }
};
</script>

<style lang="scss" scoped>
#home {
  margin-top: 200px;
}
h1 {
  font-size: 50px;
  margin-bottom: 100px;
}
.login {
  font-size: 18px;
  padding: 5px 20px;
  margin-bottom: 400px;
}
.btn-square {
  display: inline-block;
  padding: 0.5em 1em;
  text-decoration: none;
  background: #668ad8;
  color: #fff;
  border-bottom: solid 4px #627295;
  border-radius: 5px;
}
.btn-square:active {
  -webkit-transform: translateY(4px);
  transform: translateY(4px);
  border-bottom: none;
}
footer {
  width: 800px;
  margin: 0 auto;
  border-top: 1px solid #ddd;
  padding: 50px 0;
  .providedby {
    margin-top: 10px;
  }
}
</style>