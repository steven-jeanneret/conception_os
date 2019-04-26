<template>
  <div class="border cell col-auto px-0 text-center" :class="borderColor">
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
      reduction: { type: Boolean, default: false },
    },
    computed: {
      borderColor() {
        return 'border-' + this.$parent.colorComponent(this.nbArray, this.index, this.reduction)
      }
    },
  }
</script>

<style scoped>
  .cell {
    display: inline-block;
    background: rgba(45, 51, 45, 0.35);
    width: 50px;
    height: 50px;
    margin: 10px;
  }

  .cell-value {
    font-size: 20pt;

  }
</style>