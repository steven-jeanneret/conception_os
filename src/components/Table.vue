<template>
  <div>
    <div>

      <div class="my-2 table_container border border-primary">
        <cell v-for="i in slice(v1.length)" :value="v1[i]" :nb-array="0" :index="i" :key="i"/>
      </div>

    </div>
    <div>
      <div>
        <arrow v-for="i in slice(v1.length)" :key="i" class="element" :nb-array="0" :index="i"
               :parallel="parallel"/>
      </div>
    </div>
    <div>
      <div class="my-2 table_container border border-primary">
        <cell v-for="i in slice(v2.length)" :value="v2[i]" :nb-array="1" :index="i" :key="i"/>
      </div>
    </div>
    <div>
      <div>
        <arrow v-for="i in slice(v1.length)" :key="i" class="element" :nb-array="1" :index="i"
               :parallel="parallel"/>
      </div>
    </div>
    <div>
      <div class="my-2 table_container border border-primary" v-show="colorV2[0]">
        <cell v-for="i in slice(v1.length)" :key="i" class="element" :value="vResult[i]" v-show="colorV2[i]"
              :nb-array="2"
              :index="i"/>
      </div>
    </div>

    <div v-for="index in range(1,colorVReduction.length)" :key="index" :ref="index + 'reduction'">
      <div>
        <arrow v-for="i in slice(vReduction[index].length)" :key="i" class="element" :nb-array="index" :index="i"
               :parallel="parallel" :reduction="true" v-show="colorVReduction[index][i]===1"/>
      </div>
      <div class="my-2 table_container border border-primary" v-show="colorVReduction[index][0]===1">
        <cell v-for="(v, i) in vReduction[index]" :value="v" :nb-array="0" :index="i" :key="i"
              v-show="colorVReduction[index][i]===1"/>
      </div>
    </div>
    <div class="text-center">
      <button class="btn btn-primary" :disabled="animIsRunning" @click="startAnimation">{{btnText}}</button>
    </div>
  </div>
</template>

<script>
  import Cell from './Cell'
  import Vue from 'vue'
  import Arrow from './Arrow'

  const animTime = 500

  export default {
    name: 'Table',
    components: { Cell, Arrow },
    data () {
      return {
        animIsRunning: false,
        colorV1: [],
        colorV2: [],

        colorVReduction: [],
        btnText: 'Start',
        vResult: [],
        vReduction: [],
        vReductionIndex: [],
      }
    },
    methods: {
      startAnimation () {
        if(!this.animIsRunning) {
          this.animIsRunning = true
          this.btnText = 'Restart'
          this.colorV1 = []
          this.colorV2 = []
          this.colorVReduction = []

          let instance = this
          for (let i = 0; i < this.v1.length; i++) {
            setTimeout(function () {
              Vue.set(instance.colorV1, i, 1)
              setTimeout(function () {
                Vue.set(instance.colorV1, i, 2)
                Vue.set(instance.colorV2, i, 1)
                setTimeout(function () {
                  Vue.set(instance.colorV2, i, 2)
                  if (i === instance.v1.length - 1) {
                    instance.reduction()
                  }
                }, animTime)
              }, animTime)
            }, instance.timeBetweenThread(i))
          }
        }
      },

      reduction () {
        let instance = this
        let totalSleep = 0
        for (let j = 0; j < this.vReduction.length; ++j) {
          instance.colorVReduction[j] = [0]
          totalSleep += animTime
          for (let i = 0; i < instance.vReduction[j].length; ++i) {
            setTimeout(function () {
              Vue.set(instance.colorVReduction[j], i, 1)
              instance.$forceUpdate()
              if(j === instance.vReduction.length-1 && i === instance.vReduction[j].length-1) {
                instance.animIsRunning = false
              }
            }, totalSleep + instance.timeBetweenThread(i))
          }
          totalSleep += instance.timeBetweenThread(instance.vReduction[j].length)
        }
      },

      slice (n) {
        return this.range(0, n)
      },
      range (start, end) {
        if (start > end) {
          return []
        }
        return Array.from(new Array(end - start), (val, index) => index + start)
      },
      colorComponent (nbArray, index) {
        let val
        if (nbArray === 0) {
          val = this.colorV1[index]
        } else {
          val = this.colorV2[index]
        }
        return val !== 1 ? 'primary' : 'danger'
      },
      timeBetweenThread (i) {
        return (this.parallel) ? 0 : i * 500
      }
    },
    props: {
      v1: { type: Array },
      v2: { type: Array },
      parallel: { type: Boolean, default: false }
    },
    mounted () {
      this.$nextTick(() => {
        for (let i in this.slice(this.v1.length)) {
          this.vResult[i] = this.v1[i] * this.v2[i]
        }

        let moitier = this.vResult.length
        this.vReduction[0] = this.vResult
        let j = 0

        while (moitier > 1) {
          moitier = Math.round(this.vReduction[j].length / 2)
          ++j
          this.vReduction[j] = []
          for (let i = 0; i < moitier; i++) {
            this.vReduction[j][i] = this.vReduction[j - 1][i]
            if (i + moitier < this.vReduction[j - 1].length) {
              this.vReduction[j][i] += this.vReduction[j - 1][i + moitier]
            }
          }
          Vue.set(this.vReductionIndex, j, j)
        }
      })
    },
  }
</script>

<style scoped>
  .table_container {
    background: rgba(45, 51, 45, 0.15);
  }

  .element {
    display: inline-block;
  }

</style>