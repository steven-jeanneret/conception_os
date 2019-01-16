<template>
    <div class="border border-primary cell">
        <span class="cell-value">{{value}}</span>
    </div>
</template>

<script>
  export default {
    name: 'Cell',
    props: {
      value: {type: Number, default: 0}
    },
  }
</script>

<style scoped>
    .cell {
        display: inline-block;
        background: rgba(45, 51, 45, 0.35);
        width: 100px;
        height: 100px;
        padding-top: 50px;
        margin: 10px;
    }
    .cell-value {
        font-size: 20pt;
    }
</style>