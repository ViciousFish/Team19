<template>
  <div id="app">
    <nav v-if="authenticated">
      <router-link to="/" exact>Campaigns</router-link>
      <router-link to="/report">Reports</router-link>
      <router-link to="/settings">Settings</router-link>
      <a v-on:click="logout">Logout</a>
    </nav>
    <router-view v-if="authenticated" id="rootcontainer"/>
  </div>
</template>

<script>
import axios from 'axios';
import { API_URL } from './config';

const HOME_URL = API_URL.substring(0, API_URL.length - 3);

export default {
  mounted() {
    axios.get(`${API_URL}/status`, { withCredentials: true })
      .then((response) => {
        if (response.status === 200) {
          this.$store.commit('setAuthenticated', true);
        } else {
          this.$store.dispatch('login');
        }
      }).catch(() => {
        this.$store.dispatch('login');
      });
  },
  computed: {
    authenticated() {
      return this.$store.state.authenticated;
    },
  },
  methods: {
    logout() {
      window.location = `${HOME_URL}logout`;
    },
  },
};
</script>

<style lang="scss">
@import url(./animation.css);
body {
  background: #eee;
  line-height: 1.8;
  margin: 0;
  // font-family: "Avenir", Helvetica, Arial, sans-serif;
  // font-family: "Trebuchet MS", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
nav {
  color: white;
  // width: 200px;
  padding-top: 30px;
  position: fixed;
  float: left;
  height: 100vh;
  overflow: auto;
  background: #333;
  background-image: url("./assets/mountains.jpg");
  background-size: cover;
  background-position: center center;
}
nav a {
  text-decoration: none;
  width: 200px;
  padding: 10px 30px;
  color: white;
  display: block;
}
nav a:hover {
  background: rgba(75, 75, 75, 0.6);
  text-decoration: underline;
}
#rootcontainer {
  padding: 10px;
  box-sizing: border-box;
  margin-left: 260px;
  margin-right: 60px;
  overflow-x: hidden;
  overflow-y: visible;
  height: 100vh;
}
.router-link-active {
  text-decoration: underline;
  background: rgba(50, 50, 50, 0.6);
}
.button {
  cursor: pointer;
  background: #333;
  color: #eee;
  border-radius: 3px;
  -webkit-appearance: none;
  flex: 1 0 auto;
}
.button:hover {
  background: #555;
}
.btn-success {
  background: #0a0;
}
.btn-success:hover {
  background: rgb(0, 204, 0);
}

input[type="text"] {
  border-style: solid;
  border-width: 1px;
  border-color: rgb(212, 212, 212);
  border-radius: 3px;
}
</style>
