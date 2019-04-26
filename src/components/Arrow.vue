<template>
  <div>
    <span v-show="isVisible" :class="textColor"><i class="fas fa-arrow-down"></i> th. {{parallel ? index : 0}}</span>
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
        if (this.reduction) {
          return 'text-primary'
        }
        return 'text-' + this.$parent.colorComponent(this.nbArray, this.index)
      },
      isVisible () {
        if (this.reduction) {
          return true
        }
        if (this.nbArray === 0) {
          return this.$parent.colorV1[this.index]
        } else {
          return this.$parent.colorV2[this.index]
        }
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