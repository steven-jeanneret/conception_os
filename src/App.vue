<template>
  <div id="app">
    <Table :values="[3,4,5]"/>
    <Table :values="[7,8,9]"/>
  </div>
</template>

<script>
import Table from './components/Table'

export default {
  name: 'app',
  components: {
    Table
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
