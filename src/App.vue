<template>
  <div id="app container-fluid">
    <div class="container my-3 border border-secondary rounded">
      <div class="row mt-2">
          <div class="col">
            <button class="btn btn-primary float-right" data-toggle="modal" data-target="#exampleModalCenter"><i class="fas fa-info"></i></button>
            <help-modal></help-modal>
          </div>
      </div>
      <div class="row justify-content-md-center text-center">
        <div class="col-4">
          <div>
            Animation time : {{animTime}} ms
            <vue-slider v-model="animTime" :min="100" :max="5000" :interval="100"></vue-slider>
          </div>
        </div>
      </div>
      <div class="row my-3 justify-content-md-center text-center">
        <div class="col-4">
          <div>
            Number elements : {{nbElem}}
            <vue-slider v-model="nbElem" :min="1" :max="36" :interval="1"></vue-slider>
          </div>
        </div>
      </div>
      <div class="row justify-content-md-center text-center">
        <div class="col-4">
          <div>
            Number of cores : {{nbCore}}
            <vue-slider v-model="nbCore" :min="1" :max="16" :interval="1"></vue-slider>
          </div>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-lg-6 col-12">
        <Table ref="table1" :v1="v1" :v2="v2"/>
      </div>
      <div class="col-lg-6 col-12">
        <Table ref="table2" :v1="v1" :v2="v2" :parallel="true"/>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <div class="text-center">
          <button class="btn btn-primary mr-2" @click="runAll">{{buttonText}}</button>
          <button class="btn btn-primary" @click="cleanAll">Clean all</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import VueSlider from 'vue-slider-component'
  import 'vue-slider-component/theme/antd.css'

  import Table from './components/Table'
  import HelpModal from './components/HelpModal'

  const baseAnimTime = 500

  export default {
    name: 'app',
    components: {
      Table,
      VueSlider,
      HelpModal,
    },
    data () {
      return {
        v1: [],
        v2: [],
        nbElem: 10,
        buttonText: 'Start all',
        animTime: baseAnimTime,
        nbCore: 4,
      }
    },
    mounted () {
      this.fullArrayRandom(this.v1, this.nbElem)
      this.fullArrayRandom(this.v2, this.nbElem)
    },
    methods: {
      fullArrayRandom (table, nb) {
        for (let i = 0; i < nb; i++) {
          table.push(Math.floor(Math.random() * 10))
        }
      },
      runAll () {
        this.buttonText = 'Restart all'
        this.$refs['table1'].restartAnimation()
        this.$refs['table2'].restartAnimation()
      },
      cleanAll () {
        this.buttonText = 'Start all'
        this.$refs['table1'].cleanAnmiation()
        this.$refs['table2'].cleanAnmiation()
      },
    },
    watch: {
      nbElem: function(value) {
        this.v1 = []
        this.v2 = []
        this.fullArrayRandom(this.v1, value)
        this.fullArrayRandom(this.v2, value)
        this.$refs['table1'].init(this.v1, this.v2)
        this.$refs['table2'].init(this.v1, this.v2)
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
