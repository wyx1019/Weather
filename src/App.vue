<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Weather</router-link> |
      <router-link to="/about">User</router-link> |
    </div>
    <router-view />
  </div>
</template>

<style>
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  background: #c6ddf8;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
  background: #2c3e50;
  color: #e6e6e6;
}

#nav a {
  font-weight: bold;
  color: #e6e6e6;
  text-decoration: none;
  padding: 3px 1px;
  margin: 0 5px;
}

#nav a.router-link-exact-active,
#nav a:hover {
  color: #c6ddf8;
  border-bottom: solid 1px #c6ddf8;
}
</style>
