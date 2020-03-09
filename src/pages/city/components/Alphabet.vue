<template>
  <ul class="list">
    <!-- @touchstart.prevent阻止默认行为，解决手机测试的时候滚动字母表，屏幕一起滚动的问题 -->
    <li class="item"
    v-for="(item,key) of cities"
    :key="key"
    :ref="key"
    @touchstart="handleTouchStart"
    @touchmove="handleTouchMove"
    @touchend="handleTouchEnd"
    @click="handleLetterClick"
    >
    {{ key }}
    </li>
  </ul>
</template>

<script>
export default {
  name: 'CityAlphabet',
  props: {
    cities: Object
  },
  computed: {
    // 实现拖动功能，需要根据下标找到对应的字母，需要数组才能实现
    // 而cities是对象，把cities对象里的数据，放到数组里
    letters () {
      const letters = []
      for (let i in this.cities) {
        letters.push(i)
      }
      return letters
    }
  },
  data () {
    return {
      // touchStatus标识位
      touchStatus: false,
      startY: 0,
      timer: null
    }
  },
  updated () {
    // A元素距离顶部的高度，这个值是固定的，把它写在updated钩子里，如果写在handleTouchMove方法里，每次移动就会执行一次，效率低
    this.startY = this.$refs['A'][0].offsetTop
  },
  methods: {
    // 把点击到的字母传递给list组件，可以先通过事件传递给父组件city，再通过city传递给list
    handleLetterClick (e) {
      console.log(e.target.innerText)
      console.log('ok')
      this.$emit('change', e.target.innerText)
    },
    handleTouchStart () {
      this.touchStatus = true
    },
    handleTouchMove (e) {
      if (this.touchStatus) {
        if (this.timer) {
          clearTimeout(this.timer)
        }
        // 函数节流，限制一下函数执行的频率
        this.timer = setTimeout(() => {
          const touchY = e.touches[0].clientY - 79 // 滑动的时候手指距离顶部的高度
          const index = Math.floor((touchY - this.startY) / 20) // 字母下标
          if (index >= 0 && index < this.letters.length) {
            // 向父组件传递字母
            this.$emit('change', this.letters[index])
          }
        }, 16)
      }
    },
    handleTouchEnd () {
      this.touchStatus = false
    }
  }
}
</script>

<style lang="scss" scoped>
.list {
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: fixed;
  z-index: 999;
  top: 1rem;
  right: 0;
  bottom: 0;
  width: .4rem;
  .item {
    line-height: .35rem;
    text-align: center;
    color: #26a2ff;
  }
}
</style>
