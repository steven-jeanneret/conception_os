<template>
  <div class="border cell" :class="borderColor">
    <span class="cell-value">{{value}}</span>
  </div>
</template>

<script>
  export default {
    name: 'Cell',
    props: {
      value: { type: Number, default: null },
      nbArray: { type: Number, default: 0},
      index: { type: Number, default: 0},
    },
    computed: {
      borderColor() {
        return 'border-' + this.$parent.colorComponent(this.nbArray, this.index)
      }
    },
  }
</script>

<style scoped>
  .cell {
    display: inline-block;
    background: rgba(45, 51, 45, 0.35);
    width: 100px;
    height: 100px;
    padding-top: 30px;
    margin: 10px;
  }

  .cell-value {
    font-size: 20pt;
  }
</style>