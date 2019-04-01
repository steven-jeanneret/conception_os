<template>
    <div id="app container-fluid">
        <div class="row">
            <div class="col-lg-6 col-12">
                <Code :parallel="true"/>
            </div>
            <div class="col-lg-6 col-12">
                <Code :parallel="false"/>
            </div>
        </div>
        <div class="row">
            <div class="col-lg-6 col-12">
                <Table :v1="v1" :v2="v2"/>
            </div>
            <div class="col-lg-6 col-12">
                <Table :v1="v1" :v2="v2" :parallel="true"/>
            </div>
        </div>
    </div>
</template>

<script>
    import Table from './components/Table'
    import Code from './components/Code'

    export default {
        name: 'app',
        components: {
            Table,
            Code
        },
        data() {
            return {
                v1: [],
                v2: [],
                nbElem: 10,
            }
        },
        mounted () {
            this.fullArrayRandom(this.v1, this.nbElem)
            this.fullArrayRandom(this.v2, this.nbElem)
        },
        methods: {
            fullArrayRandom(table, nb) {
                for (let i = 0; i < nb; i++) {
                    table.push(Math.floor(Math.random()*10))
                }
            }
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
    }
</style>
