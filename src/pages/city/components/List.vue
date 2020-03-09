<template>
  <div class="list" ref="wrapper">
    <div>
      <div class="area">
        <div class="title border-topbottom">当前城市</div>
        <div class="button-list">
          <div class="button-wrapper">
            <div class="button">{{ this.$store.state.city }}</div>
          </div>
        </div>
      </div>
      <div class="area">
        <div class="title border-topbottom">热门城市</div>
        <div class="button-list">
          <div class="button-wrapper" v-for="item of hot" :key="item.id" @click="handleCityClick(item.name)">
            <div class="button">{{item.name}}</div>
          </div>
        </div>
      </div>

      <div class="area" v-for="(item, key) of cities" :key="key" :ref="key">
        <div class="title border-topbottom"> {{key}} </div>
        <div class="item-list">
          <div class="item border-bottom" v-for="innerItem of item" :key="innerItem.id" @click="handleCityClick(innerItem.name)">
            {{innerItem.name}}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Bscroll from 'better-scroll'
export default {
  name: 'CityList',
  data () {
    return {
      currentCity: '缙云1'
    }
  },
  props: {
    hot: Array,
    cities: Object,
    letter: String
  },
  methods: {
    handleCityClick (city) {
      // 触发一个名叫changeCity的action，city作为第二个参数传入，需要在store里写个名叫changeCity的actions
      this.$store.commit('changeCity', city)
      // 编程式导航，使用js实现页面跳转
      this.$router.push('/')
    }
  },
  watch: {
    // 监听letter的变化
    letter () {
      if (this.letter) {
        // this.$refs[this.letter]是数组，不是标准的dom元素，需要通过[0]把它变成dom元素
        const element = this.$refs[this.letter][0]
        this.scroll.scrollToElement(element)
      }
    }
  },
  mounted () {
    const options = {
      scrollY: true,
      scrollX: false,
      mouseWheel: true,
      click: true,
      taps: true
    }
    this.scroll = new Bscroll(this.$refs.wrapper, options)
  }
}
</script>

<style lang="scss" scoped>
.border-topbottom {
  &:before {
    border-color: #ccc;
  }
  &:after {
    border-color: #ccc;
  }
}
.border-bottom {
  &:before {
    border-color: #ccc;
  }
}
.list {
  overflow: hidden;
  position: absolute;
  top: 1.58rem;
  left: 0;
  right: 0;
  bottom: 0;
  .title {
    line-height: .54rem;
    background: #eee;
    padding-left: .2rem;
    color: #666;
    font-size: .26rem;
  }
  .button-list {
    overflow: hidden;
    padding: .1rem .6rem .1rem .1rem;
    background-color: #fff;
    .button-wrapper {
      float: left;
      width: 33.33%;
      .button {
        margin: .1rem;
        padding: .1rem 0;
        text-align: center;
        border: .02rem solid #ccc;
        border-radius: .06rem;
      }
    }
  }
  .item-list {
    background-color: #fff;
    .item {
      line-height: .76rem;
      padding-left: .2rem;
    }
  }
}
</style>
