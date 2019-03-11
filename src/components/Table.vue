<template>
    <div>
        <div class="col-6">
            <div class="my-2 table_container border border-primary" :style="{width: sizeT}">
                <cell v-for="i in slice(v1.length)" :value="v1[i]" :nb-array="0" :index="i" :key="i"/>
            </div>
        </div>
        <div class="col-6">
            <div :style="{width: sizeT}">
                <arrow v-for="i in slice(v1.length)" :key="i" class="element" :nb-array="0" :index="i"
                       :parallel="parallel"/>
            </div>
        </div>
        <div class="col-6">
            <div class="my-2 table_container border border-primary" :style="{width: sizeT}">
                <cell v-for="i in slice(v2.length)" :value="v2[i]" :nb-array="1" :index="i" :key="i"/>
            </div>
        </div>
        <div class="col-6">
            <div :style="{width: sizeT}">
                <arrow v-for="i in slice(v1.length)" :key="i" class="element" :nb-array="1" :index="i"
                       :parallel="parallel"/>
            </div>
        </div>
        <div class="col-6">
            <div class="my-2 table_container border border-primary" :style="{width: sizeT}" v-show="show2[0]">
                <div v-for="i in slice(v1.length)" :key="i" class="element">
                    <cell :key="i" :value="v3[i]" v-show="show2[i]" :nb-array="2" :index="i"/>
                </div>
            </div>
        </div>
        <div class="col-6">
            <button @click="startAnimation">{{btnText}}</button>
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
        components: {Cell, Arrow},
        data() {
            return {
                sizeTable: 0,
                animHasRunned: false,
                show: [],
                show2: [],
                btnText: 'Start',
                v3: [],
            }
        },
        methods: {
            startAnimation() {
                if (this.animHasRunned) {
                    this.btnText = 'Start'
                    this.show = []
                    this.show2 = []
                } else {
                    this.btnText = 'Clear'
                    let instance = this
                    for (let i = 0; i < this.v1.length; i++) {
                        setTimeout(function () {
                            Vue.set(instance.show, i, 1)
                            setTimeout(function () {
                                Vue.set(instance.show, i, 2)
                                Vue.set(instance.show2, i, 1)
                                setTimeout(function () {
                                    Vue.set(instance.show2, i, 2)
                                }, animTime)
                            }, animTime)
                        }, (instance.parallel) ? 0 : i * 1000)
                    }
                }
                this.animHasRunned = !this.animHasRunned
            },
            slice(n) {
                return Array.from(new Array(n), (val, index) => index)
            },
            colorComponent(nbArray, index) {
                let val
                if (nbArray === 0) {
                    val = this.show[index]
                } else {
                    val = this.show2[index]
                }
                return val !== 1 ? 'primary' : 'danger'
            },
        },
        props: {
            v1: {type: Array},
            v2: {type: Array},
            parallel: {type: Boolean, default: false}
        },
        computed: {
            sizeT() {
                return this.sizeTable + 'px'
            },
        },
        mounted() {
            this.sizeTable = this.v1.length * 120 + 20
            for(let i in this.slice(this.v1.length)){
                this.v3[i] = this.v1[i] * this.v2[i]
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