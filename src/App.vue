<template>
  <div id="app">
    <div class="container-fluid">
        <div class="row bg-dark">
            <div class="col-lg-12">
                <p class="text-center text-light display-4 pt-2"
                style="font-size: 25px;">Crud-Vue-Basic</p>
            </div>
        </div>
    </div>
    <div id="nav">
      <router-link to="/">Inicio</router-link> |
      <router-link to="/client">Clientes</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }

  #nav {
    padding: 30px;
  }

  #nav a {
    font-weight: bold;
    color: #2c3e50;
  }

  #nav a.router-link-exact-active {
    color: #42b983;
  }
</style>
