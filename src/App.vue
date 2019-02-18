<template>
    <div id="app">
        <div class="row">
            <div class="col-6">
                <Table :v1="[3,4,5]" :v2="[7,8,9]"/>
            </div>
            <div class="col-6">
                <Table :v1="[1,6,8]" :v2="[6,4,1]" :parallel="true"/>
            </div>
        </div>

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
