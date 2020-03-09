<template>
  <div @click="handleCityClick">
    <div class="search">
      <input type="text" v-model="keyword" name="" id="" class="search-input" placeholder="输入城市名或拼音">
    </div>
    <div class="search-content" ref="search" v-show="keyword" @click="handleCityClick">
      <ul>
        <li
          class="search-item border-bottom"
          v-for="item of list"
          :key="item.id"
          @click="handleCityClick(item.name)"
        >
          {{item.name}}
        </li>
        <li class="search-item border-bottom" v-show="hasNoData" @click="handleCityClick">
          没有找到匹配数据22
        </li>
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
      // 用于节流
      timer: null
    }
  },
  computed: {
    hasNoData () {
      return !this.list.length
    }
  },
  methods: {
    handleCityClick (city) {
      console.log(this)
      // 触发一个名叫changeCity的action，city作为第二个参数传入，需要在store里写个名叫changeCity的actions
      // this.$store.dispatch('changeCity', city)
      // this.$store.commit('changeCity', city)
      // this.changeCity(city)
      // 编程式导航，使用js实现页面跳转
      // this.$router.push('/')
    }
  },
  watch: {
    // 监听keyword的改变
    keyword () {
      // 节流
      if (this.timer) {
        clearTimeout(this.timer)
      }
      if (!this.keyword) {
        this.list = []
        return
      }
      this.timer = setTimeout(() => {
        const result = []
        for (let i in this.cities) {
          this.cities[i].forEach((value) => {
            // 当拼音匹配或者名字匹配，就把数据纳入到结果
            if (value.spell.indexOf(this.keyword) > -1 || value.name.indexOf(this.keyword) > -1) {
              result.push(value)
            }
          })
        }
        this.list = result
      }, 100)
    }
  },
  mounted () {
    // 如果搜索结果很多，超出一页范围，就让搜索区域可以滚动
    this.scroll = new Bscroll(this.$refs.search)
  }
}
</script>

<style lang="scss" scoped>
.search {
  height: .72rem;
  padding: 0 .1rem;
  background: #26a2ff;
  .search-input {
    box-sizing: border-box;
    width: 100%;
    height: .62rem;
    padding: 0 .1rem;
    line-height: .62rem;
    text-align: center;
    border-radius: .06rem;
    color: #666;
  }
}
.search-content {
  z-index: 1;
  overflow: hidden;
  position: absolute;
  top: 1.58rem;
  left: 0;
  right: 0;
  bottom: 1rem;
  background: #eee;
  .search-item {
    line-height: .62rem;
    padding-left: .2rem;
    background: #fff;
    color: #666;
  }
}
</style>
