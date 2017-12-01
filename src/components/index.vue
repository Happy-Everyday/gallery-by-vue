<template>
    <div class="index_page">
        <div class="stage" id="stage">
            <div class="pic_item" v-for="(item, index) of list" :style="item.style" :class="{'center': item.isCenter, 'is-inverse': item.isInter}" @click="changePis(index)">
                <img :src="item.src" alt="">
                <p>{{item.title}}</p>
                <div class="img-back">
                    <p>{{item.desc}}</p>
                </div>   
            </div>
        </div>
        <div class="controller">
            <a  v-for="(item, index) of list" :class="{'center': item.isCenter, 'is-inverse': item.isInter}" @click="changePis(index)"></a>
        </div>
    </div>
</template>
<style scoped lang='sass'>
    @import './index.scss';
</style>
<script>
    import imgDataArr from '../data/imgData.js'
    export default {

        components: {
        },
        props: {
        },
        data () {
            return {
                list: [],
                style: '',
                constant: {
                    centerPos: { // 中心点位置
                        left: 0,
                        top: 0
                    },
                    xPosRange: { // 左右两边的取值范围
                        leftRange: [0, 0], // 左边的取值范围
                        rightRange: [0, 0],// 右边的取值范围
                        y: [0, 0] // Y轴的取值范围
                    },
                    yPosRange: { // 上边的取值范围
                        topRange: [0, 0], // 左边的取值范围
                        x: [0, 0] // X轴的取值范围
                    }
                }
            }
        },
        created () {
        },
        mounted () {
            let _this = this
            // 先划分好三块区域的图片取值范围，上，左，右

            // 获取舞台大小
            let stageWidth = document.getElementById('stage').clientWidth
            let stageHeight = document.getElementById('stage').clientHeight

            // 获取一个一个pic_item的宽高
            let picItemWidth = 320
            let picItemHeight = 360

            this.constant.centerPos = { // 中心点位置为舞台宽高一半减去图片宽高的一半
                left: stageWidth/2 - picItemWidth/2,
                top: stageHeight/2 - picItemHeight/2
            }
            this.constant.xPosRange = { // 水平方向的取值范围要去掉中间图片那一块范围，不然造成遮挡，影响美观
                leftRange: [-picItemWidth/2, stageWidth/2 - picItemWidth/2*3],
                rightRange: [stageWidth/2 + picItemWidth/2, stageWidth - picItemWidth/2],
                y: [-picItemHeight/2, stageHeight-picItemHeight/2]
            }
            this.constant.yPosRange = {
                topRange: [ -picItemHeight/2, stageHeight/2 - picItemHeight/2*3],
                x: [stageWidth/2-picItemWidth/2, stageWidth/2]
            }

            // 将数组进行重组,加入图片链接地址
            this.list = imgDataArr.map(function(item, index) {
                return {
                    src: require(`../assets/images/${item.fileName}`),
                    title: item.title,
                    desc: item.desc,
                    isCenter: index === 0? true: false,
                    isInter: false,
                    style: `transform: rotate(0deg);left: ${_this.constant.centerPos.left}px;top:${_this.constant.centerPos.top}px;`
                }
            })
            let timer = setTimeout(function() {
                _this.getPicPos(0)
            }, 200)
        },
        watch: {
        },
        methods: {
            getChange (value) {
                console.log(value)
            },
            getRandomNum(small, big) { // 取两个值之间的随机值
                return Math.floor(Math.random()*(big - small) + small)
            },
            getRandom30Deg () { // 取正负30度之间的随机值
                return ((Math.random() > 0.5 ? '' :'-') + Math.floor(Math.random() * 30))
            },
            changePis (centerIndex) {
                if (this.list[centerIndex].isCenter) {
                    this.getPicInverse(centerIndex)
                } else {
                    this.getPicPos(centerIndex)
                }
            },
            getPicInverse (centerIndex) {
                this.list[centerIndex].isInter = !this.list[centerIndex].isInter
            },
            getPicPos (centerIndex) { // 获取图片的各个随即位置
                let _this = this
                // 上部布局，随机取值，取0或1个
                let topPicNum = Math.floor(Math.random()*2)

                // 将数组拆分，分成左，上，右三个数组

                // 上部图片数组
                let topImgSpliceIndex = Math.ceil(Math.random() * (this.list.length - topPicNum))

                // 左右图片数组
                for(let i = 0,len = this.list.length,k = len / 2;i < len;i++) {
                    this.list[i].isCenter = false
                    this.list[i].isInter = false
                    if (i < k) {
                        this.list[i].style = `transform: rotate(${_this.getRandom30Deg()}deg);left:${_this.getRandomNum(_this.constant.xPosRange.leftRange[0], _this.constant.xPosRange.leftRange[1])}px;top:${_this.getRandomNum(_this.constant.xPosRange.y[0], _this.constant.xPosRange.y[1])}px;`
                    } else {
                        this.list[i].style = `transform: rotate(${_this.getRandom30Deg()}deg);left:${_this.getRandomNum(_this.constant.xPosRange.rightRange[0], _this.constant.xPosRange.rightRange[1])}px;top:${_this.getRandomNum(_this.constant.xPosRange.y[0], _this.constant.xPosRange.y[1])}px;`
                    }
                    if (i === centerIndex) {
                        this.list[i].isCenter = true
                        this.list[i].style = `left: ${this.constant.centerPos.left}px;top:${this.constant.centerPos.top}px;`
                    }
                    if (i === topImgSpliceIndex && topPicNum > 0  && i !== centerIndex) {
                        this.list[i].style = `transform: rotate(${_this.getRandom30Deg()}deg);top:${_this.getRandomNum(_this.constant.yPosRange.topRange[0], _this.constant.yPosRange.topRange[1])}px;left:${_this.getRandomNum(_this.constant.yPosRange.x[0], _this.constant.yPosRange.x[1])}px;`
                    }
                }

            }
        }
    }

</script>