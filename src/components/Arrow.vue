<template>
  <div>
    <span v-show="isVisible" :class="textColor"><i class="fas fa-arrow-down"></i> thread {{parallel ? i : 0}}</span>
  </div>
</template>

<script>
  export default {
    name: 'Arrow',
    props: {
      nbArray: { type: Number, default: 0},
      index: { type: Number, default: 0},
      parallel: { type: Boolean, default: false},
    },
    computed: {
      textColor() {
        return 'text-' + this.$parent.colorComponent(this.nbArray, this.index)
      },
      isVisible() {
        if(this.nbArray === 0){
          return this.$parent.show[this.index]
        } else {
          return this.$parent.show2[this.index]
        }
      }
    }
  }
</script>

<style scoped>

</style>