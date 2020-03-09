<template>
  <div class="header">
    城市选择
    <router-link to="/">
      <div class="mui-icon mui-icon-back"></div>
    </router-link>
  </div>
</template>

<script>
export default {
  name: 'CityHeader'
}
</script>

<style lang="scss" scoped>
.header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 999;
  overflow: hidden;
  height: .8rem;
  line-height: .8rem;
  text-align: center;
  color: #fff;
  background: #26a2ff;
  font-size: .32rem;
  .mui-icon-back {
    position: absolute;
    top: .16rem;
    left: 0;
    width: .64rem;
    text-align: center;
    font-size: .4rem;
    color: #fff;
  }
}
</style>
