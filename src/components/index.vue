<template>
    <div class="index_page">
        <div class="stage">
            <div class="pic_item" :style="item.style" v-for="item of list" :key='item.name' :class="{'center': item.center}">
                <img :src="item.src" alt="">
                <p>{{item.name}}</p>       
            </div>
        </div>
        <div class="controller">
        </div>
    </div>
</template>
<style scoped lang='sass'>
    @import './index.scss';
</style>
<script>
    import imgData from '../data/imgData.js'
    export default {

        components: {
        },
        props: {
        },
        data () {
            return {
                list: [],
                style: ''
            }
        },
        created () {
            let indexNum = Math.floor(Math.random()*imgData.length)
            this.list = imgData.map((item) => {
                return {
                    src: require(`../assets/images/${item.name}`),
                    name: item.name,
                    title: '图片'
                }
            })
            this.list.forEach((item, index) => {
                item.rotateNum = Math.random() > 0.5? Math.random()*30: -Math.random()*30
                item.posLeftNum = Math.random()*100
                item.posTopNum = Math.random()*100
                item.style = `transform: rotate(${item.rotateNum}deg) translate(-50%, -50%);top:${item.posTopNum}%;left:${item.posLeftNum}%;`
                if (index === indexNum) {
                    item.style = ''
                    item.center = true
                } else {
                    item.center = false                    
                }
            })
        },
        mounted () {
        },
        watch: {
        },
        methods: {
            getChange (value) {
                console.log(value)
            }
        }
    }

</script>