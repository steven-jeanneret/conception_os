<template>
    <div class="my-2 table_container border border-primary" :style="{width: sizeT}">
        <cell v-for="value in values" :value="value"/>
    </div>
</template>

<script>
  import Cell from './Cell'

  export default {
    name: 'Table',
    components: { Cell },
    data () {
      return {
        sizeTable: 0,
      }
    },
    props: {
      values: { type: Array },
    },
    computed: {
      sizeT () {
        return this.sizeTable + 'px'
      }
    },
    mounted () {
      this.sizeTable = this.values.length * 140
    },
  }
</script>

<style scoped>
    .table_container {
        background: rgba(45, 51, 45, 0.15);
    }
</style>