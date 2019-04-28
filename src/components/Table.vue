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
        <arrow v-for="i in slice(v1.length)" :key="i" class="element" :nb-array="2" :index="i"
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
        <cell v-for="(v, i) in vReduction[index]" :value="v" :nb-array="index" :index="i" :key="i"
              v-show="colorVReduction[index][i]===1" :reduction="true"/>
      </div>
    </div>
    <div class="text-center">
      <button class="btn btn-primary mr-2" :disabled="animIsRunning" @click="restartAnimation">{{btnText}}</button>
      <button class="btn btn-primary" :disabled="animIsRunning" @click="cleanAnmiation">Clean</button>
    </div>
  </div>
</template>

<script>
  import Cell from './Cell'
  import Vue from 'vue'
  import Arrow from './Arrow'

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
      cleanAnmiation () {
        this.colorV1 = []
        this.colorV2 = []
        this.colorVReduction = []
        this.btnText = 'Start'
        this.animIsRunning = false
      },
      startAnimation () {
        if (!this.animIsRunning) {
          this.animIsRunning = true
          this.btnText = 'Restart'
          for (let i = 0; i < this.v1.length; i++) {
            let sequenceCore = (this.parallel ? Math.floor(i / this.nbCore) : i)
            setTimeout(() => {
              Vue.set(this.colorV1, i, 1) //Color in red first cell + first arrow + second cell
            }, (this.animTime * 2 * sequenceCore))
            setTimeout(() => {
              Vue.set(this.colorV1, i, 2) //Remove red on first cell + first arrow + second cell
              Vue.set(this.colorV2, i, 1) //Color in red second cell + second arrow + third cell
            }, (this.animTime * 2 * sequenceCore + this.animTime))
            setTimeout(() => {
              Vue.set(this.colorV2, i, 2) //Remove red on second cell + second arrow + third cell
              if (i === this.v1.length - 1) {
                this.reduction() //If last cell reduce
              }
            }, (this.animTime * 2 * sequenceCore + this.animTime * 2))
          }
        }
      },
      restartAnimation () {
        this.cleanAnmiation()
        this.startAnimation()
      },
      reduction () {
        let totalSleep = 0
        for (let j = 0; j < this.vReduction.length; ++j) {
          this.colorVReduction[j] = [0]
          let timeSleep = 0
          for (let i = 0; i < this.vReduction[j].length; ++i) {
            timeSleep = ((this.parallel ? (Math.floor(i / this.nbCore)) : i) + 1) * this.animTime
            setTimeout(() => {
              Vue.set(this.colorVReduction[j], i, 1)
              this.$forceUpdate() //Force redraw else redraw when all cell are finished
              if (j === this.vReduction.length - 1 && i === this.vReduction[j].length - 1) {
                this.animIsRunning = false //Last cell draw, anim is finished
              }
            }, totalSleep + timeSleep)
          }
          totalSleep += timeSleep
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
      colorComponent (nbArray, index, isReduction = false) {
        let val
        if (isReduction) {
          val = this.colorVReduction[nbArray][index]
        } else if (nbArray === 0) {
          val = this.colorV1[index]
        } else if (nbArray === 1) {
          if (this.colorV1[index] === 1 || this.colorV2[index] === 1) {
            val = 1
          } else {
            val = this.colorV1[index] || this.colorV2[index]
          }
        } else {
          val = this.colorV2[index]
        }
        return val === 1 ? 'danger' : 'primary'
      },
      init (v1, v2) {
        this.v1 = v1
        this.v2 = v2
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
          this.cleanAnmiation()
        }
      }
    },
    computed: {
      animTime () {
        return this.$parent.animTime
      },
      nbCore () {
        return this.$parent.nbCore
      }
    },
    props: {
      v1: { type: Array },
      v2: { type: Array },
      parallel: { type: Boolean, default: false }
    },
    mounted () {
      this.$nextTick(() => {
        this.init()
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