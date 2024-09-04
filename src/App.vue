<template>
  <div id="app">
    <PersonTable/>
  </div>
</template>

<script>
import PersonTable from './components/personTable.vue';

export default {
  name: 'App',
  components: {
    PersonTable
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
