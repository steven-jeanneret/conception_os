<template>
  <div>
    <span v-show="isVisible" class="ml-2" :class="textColor"><i class="fas fa-arrow-down"></i> {{parallel ? index%$parent.nbCore : 0}}</span>
  </div>
</template>

<script>
  export default {
    name: 'Arrow',
    props: {
      nbArray: { type: Number, default: 0 },
      index: { type: Number, default: 0 },
      parallel: { type: Boolean, default: false },
      reduction: { type: Boolean, default: false },
    },
    computed: {
      textColor () {
        // if (this.reduction) {
        //   return 'text-primary'
        // }
        return 'text-' + this.$parent.colorComponent(this.nbArray, this.index, this.reduction)
      },
      isVisible () {
        if (this.reduction) {
          return true
        }
        if (this.nbArray === 0) {
          return this.$parent.colorV1[this.index]
        }
        return this.$parent.colorV2[this.index]
      }
    }
  }
</script>

<style scoped>
  div {
    display: inline-block;
    margin: 0 10px;
    width: 50px;
  }
</style>