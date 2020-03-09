<template>
  <div>
    <div class="header">
      <div class="header-input">
        <span class="mui-icon mui-icon-search"></span>输入城市/景点/游玩主题
      </div>
      <router-link to="/city">
        <div class="header-right">
          {{ this.$store.state.city }}
          <span class="mui-icon mui-icon-arrowdown"></span>
        </div>
      </router-link>
    </div>
    <!-- 轮播图区域 -->
    <swiper :lunbotuList="lunbotuList" :isfull="true"></swiper>

    <!-- 九宫格 到 6宫格 的改造工程 -->
    <ul class="mui-table-view mui-grid-view mui-grid-9">
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <router-link to="/home/newslist">
          <img src="../../assets/images/menu1.png" alt>
          <div class="mui-media-body">新闻资讯</div>
        </router-link>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <router-link to="/home/photolist">
          <img src="../../assets/images/menu2.png" alt>
          <div class="mui-media-body">图片分享</div>
        </router-link>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <router-link to="/home/goodslist">
          <img src="../../assets/images/menu3.png" alt>
          <div class="mui-media-body">商品购买</div>
        </router-link>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <a href="#">
          <img src="../../assets/images/menu4.png" alt>
          <div class="mui-media-body">留言反馈</div>
        </a>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <a href="#">
          <img src="../../assets/images/menu5.png" alt>
          <div class="mui-media-body">视频专区</div>
        </a>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <a href="#">
          <img src="../../assets/images/menu6.png" alt>
          <div class="mui-media-body">联系我们</div>
        </a>
      </li>
    </ul>

    <div>
      <div class="title">热销推荐</div>
      <ul>
        <router-link
          tag="li"
          class="item border-bottom"
          v-for="item of recommendList"
          :key="item.id"
          :to="'/detail/' + item.id"
        >
          <img class="item-img" :src="item.imgUrl" />
          <div class="item-info">
            <p class="item-title">{{item.title}}</p>
            <p class="item-desc">{{item.desc}}</p>
            <button class="item-button">查看详情</button>
          </div>
        </router-link>
      </ul>
    </div>
  </div>
</template>

<script>
import swiper from '@/common/swiper.vue'
import axios from 'axios'
import { Toast } from 'mint-ui'

export default {
  name: 'HomeContainer',
  data () {
    return {
      lunbotuList: [], // 保存轮播图的数组
      recommendList: [],
      weekendList: []
    }
  },
  created () {
    this.getLunbotu()
    this.getHomeInfo()
  },
  methods: {
    getLunbotu () {
      // 获取轮播图数据的方法
      axios.get('http://www.liulongbin.top:3005/api/getlunbo').then(this.getLunboSucc)
    },
    getLunboSucc (res) {
      if (res.data.status === 0) {
        // 成功了
        this.lunbotuList = res.data.message
      } else {
        // 失败的
        Toast('加载轮播图失败。。。')
      }
    },
    getHomeInfo () {
      // axios.get返回一个promise对象
      axios.get('/api/index.json?city=' + this.city).then(this.getHomeInfoSucc)
    },
    getHomeInfoSucc (res) {
      console.log(res)
      res = res.data
      if (res.ret && res.data) {
        const data = res.data
        this.recommendList = data.recommendList
        this.weekendList = data.weekendList
      }
    }
  },
  components: {
    swiper
  }
}
</script>

<style lang="scss" scoped>
.header {
  // position: fixed;
  // width: 100%;
  // top: 0;
  // left: 0;
  // z-index: 999;
  display: flex;
  line-height: 40px;
  background: #26a2ff;
  color: #fff;
  .header-input {
    flex: 1;
    height: .8rem;
    line-height: .8rem;
    padding-left: .2rem;
    background: #fff;
    border-radius: .1rem;
    color: #ccc;
  }
  .header-right {
    min-width: 1.04rem;
    padding: 0 .1rem;
    float: right;
    text-align: center;
    color: #fff;
  }
}
.mui-grid-view.mui-grid-9 {
  background-color: #fff;
  border: none;
  img {
    width: 60px;
    height: 60px;
  }

  .mui-media-body {
    font-size: 13px;
  }
}

.mui-grid-view.mui-grid-9 .mui-table-view-cell {
  border: 0;
}

.title{
  margin-top: .2rem;
  line-height: .8rem;
  background: #eee;
  text-indent: .2rem;
}
.item {
  background-color: #fff;
  overflow: hidden;
  display: flex;
  height: 1.9rem;
  .item-img {
    width: 1.7rem;
    height: 1.7rem;
    padding: .1rem;
  }
  .item-info {
    flex: 1;
    padding: .1rem;
    // 小技巧，加了这个，才会出现省略号
    min-width: 0;
    .item-title {
      line-height: .54rem;
      font-size: .32rem;
      overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
      margin-bottom: 0px;
    }
    .item-desc {
      line-height: .4rem;
      color: #ccc;
      overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
      margin-bottom: 0px;
    }
    .item-button {
      line-height: .44rem;
      margin-top: .16rem;
      background: #ff9300;
      padding: 0 .2rem;
      border-radius: .06rem;
      color: #fff;
      margin-bottom: 0px;
    }
  }
}
</style>
