<template>
  <nav class="global-nav">
    <h1 class="global-nav-item">Til Note</h1>
    <div class="global-nav-item mr-0" v-if="user">
      <span class="userName">{{ user.displayName }}</span>
      <button class="logoutBtn" @click="logout">Logout</button>
    </div>
  </nav>
</template>

<script>
export default {
  name: "global-nav",
  props: ["user"],
  methods: {
    logout: function() {
      firebase.auth().signOut();
    }
  }
};
</script>

<style lang="scss" scoped>
.global-nav {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 65px;
  display: flex;
  align-items: center;
  flex-wrap: nowrap;
  z-index: 32;
  padding: 16px;
  font-size: 16px;
  line-height: 1.5;
  color: #ffff;
  background-color: #29a972;
}
h1 {
  font-size: 30px;
  display: inline-block;
}
.global-nav-item {
  display: flex;
  align-items: center;
  flex-wrap: nowrap;
  margin-right: 16px;
  align-self: stretch;
}
.userName {
  margin-right: 20px;
}
.mr-0 {
  margin-left: auto;
}
</style>