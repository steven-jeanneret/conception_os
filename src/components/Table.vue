<template>
    <div>
        <div class="col-6">
            <div class="my-2 table_container border border-primary" :style="{width: sizeT}">
                <cell v-for="value in v1" :value="value" :key="value"/>
            </div>
        </div>
        <div class="col-6">
            <div :style="{width: sizeT}">
                <div v-for="i in slice(v1.length)" :key="i" class="element">
                    <i class="fas fa-arrow-down" v-show="show[i]"></i>
                </div>
            </div>
        </div>
        <div class="col-6">
            <div class="my-2 table_container border border-primary" :style="{width: sizeT}">
                <cell v-for="value in v2" :value="value" :key="value"/>
            </div>
        </div>
        <div class="col-6">
            <button @click="startAnimation">Start</button>
        </div>
    </div>
</template>

<script>
  import Cell from './Cell'
  import Vue from 'vue'

  export default {
    name: 'Table',
    components: { Cell },
    data () {
      return {
        sizeTable: 0,
        animRunning: false,
        show: [],
      }
    },
    methods: {
      startAnimation () {
        let instance = this
        for (let i = 0; i < this.v1.length; i++) {
          setTimeout(function () {
            Vue.set(instance.show, i, true)
          }, (instance.parallel) ? 0 : i * 1000)
        }
      },
      slice (n) {
        return Array.from(new Array(n), (val, index) => index)
      }
    },
    props: {
      v1: { type: Array },
      v2: { type: Array },
      parallel: {type: Boolean, default: false}
    },
    computed: {
      sizeT () {
        return this.sizeTable + 'px'
      },
    },
    mounted () {
      this.sizeTable = this.v1.length * 140
      for (let i = 0; i < this.v1.length; i++) {
        this.show.push(false)
      }
    },
  }
</script>

<style scoped>
    .table_container {
        background: rgba(45, 51, 45, 0.15);
    }

    .element {
        width: 120px;
        display: inline-block;
    }
</style>