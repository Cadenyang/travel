<template>
    <div>
        <div class="search">
            <input v-model="keyword" type="text" class="search-input" placeholder="请输入城市或者拼音"/>
        </div>
        <div v-show="keyword" class="search-content" ref="search">
            <ul>
                <li class="search-item border-bottom" v-for="item of list" :key="item.id" @click="handleCity(item.name)">{{ item.name }}</li>
                <li class="search-item border-bottom" v-show="hasNoData">暂无数据</li>
            </ul>
        </div>
    </div>
</template>
<script>
import Bscroll from 'better-scroll'

export default {
    name: 'CitySearch',
    props: {
        cities: Object
    },
    data () {
        return {
            keyword: '',
            list: [],
            timer: null
        }
    },
    watch: {
        keyword () {
            if(this.timer) clearTimeout(this.timer)
            if(!this.keyword) {
                this.list = []
                return
            }
            this.timer = setTimeout(() => {
                const result = []
                for(let i in this.cities) {
                    this.cities[i].forEach((value) => {
                        if(value.spell.indexOf(this.keyword) > -1 || value.name.indexOf(this.keyword) > -1){
                            result.push(value)
                        }
                    });
                }
                this.list = result
            },100)
        }
    },
    mounted () {
        this.scroll = new Bscroll(this.$refs.search)
    },
    computed: {
        hasNoData () {
             return !this.list.length
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
    .search {
        height: .72rem
        padding: 0 .1rem
        background: $bgColor
    }
    .search-input {
        box-sizing: border-box
        width: 100%
        height: .62rem
        line-height: .62rem
        text-align: center
        padding: 0 .2rem
        border-radius: .06rem
        color:#666
    }
    .search-content {
        z-index: 1
        overflow: hidden
        position: absolute
        top: 1.58rem 
        left: 0
        right: 0
        bottom: 0
        background: #eee
    }
    .search-item {
        line-height: .62rem
        padding-left: .2rem
        color: #666
        background: #fff
    }
</style>

