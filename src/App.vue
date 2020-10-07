<template>
  <div id="app">
    <!-- <div id="nav">
      <router-link to="/">Weather</router-link> |
      <router-link to="/about">About</router-link> |
    </div> -->
    <router-view />
  </div>
</template>

<style>
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff;
}
</style>
