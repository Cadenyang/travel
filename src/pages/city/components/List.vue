<template>
    <div class="list" ref="wrapper">
        <div>
            <div class="area">
                <div class="title border-topbottom">您的位置</div>
                    <div class="button-list">
                        <div class="button-wipper">
                            <div class="button">{{ this.$store.state.city }}</div>
                        </div>
                    </div>
            </div>
            <div class="area">
                <div class="title border-topbottom">热门城市</div>
                    <div class="button-list">
                        <div class="button-wipper" v-for="item of hot" :key="item.id" @click="handleCity(item.name)">
                            <div class="button">{{ item.name }}</div>
                        </div>
                    </div>
            </div>
            <div class="area" v-for="(item, key) of cities" :key="key" :ref="key">
                <div class="title border-topbottom">{{ key }}</div>
                    <div class="item-list">
                        <div 
                            class="item border-bottom" 
                            v-for="innerItem of item" 
                            :key="innerItem.id"
                            @click="handleCity(innerItem.name)"
                        >{{ innerItem.name}}</div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import Bscroll from 'better-scroll'

export default {
    name: 'CityList',
    props: {
        hot: Array,
        cities: Object,
        letter: String
    },
    mounted () {
        this.scroll = new Bscroll(this.$refs.wrapper)
    },
    watch: {
        letter () {
            const element = this.$refs[this.letter][0]
            if(this.letter) this.scroll.scrollToElement(element)
        }
    },
    methods: {
        handleCity (city) {
            this.$store.commit('changeCity', city)
            this.$router.push('/')
        }
    }
}
</script>
<style lang="stylus" scoped>
    @import '~@/assets/css/varibles.styl'
    .border-topbottom {
        &:before {
            border-color: #ccc
        }
        &:after {
            border-color: #ccc
        }
    }
    .border-bottom {
        &:before {
            border-color: #ccc
        }
    }
    .list {
        overflow: hidden
        position: absolute
        top: 1.58rem
        left: 0
        right: 0
        bottom: 0
    }
    .title {
        line-height: .6rem 
        background: #eee
        padding-left: .2rem
        color: #666
        font-size: .26rem
    }
    .button-list {
        padding: .1rem .6rem .1rem .1rem
        overflow: hidden
    }
    .button-wipper {
        float: left
        width: 33.33%
    }
    .button {
        padding: .1rem 0
        margin: .1rem
        text-align: center
        border: .02rem solid #ccc
        border-radius: .06rem
    }
    .item {
        line-height: .76rem
        padding-left: .2rem
    }
</style>

