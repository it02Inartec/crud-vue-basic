<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      CRUD de clientes, usando Vue.js, Laravel 7, vue-axios, PostgreSQL & Bootstrap 4
    </p>
    <a href="https://github.com/it02Inartec/crud-vue-basic" target="_blank">Ver código en github</a>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>
