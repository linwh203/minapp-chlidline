<template>
  <div class="container">
    <div class="map-sub">
      <div class="map-sub-line"></div>
      <div class="map-sub-btn">
        <div class="map-sub-btn-in" @click="locate">
          <img src="../../assets/icon-map-gps.png" alt="" class="btn-show">
        </div>
      </div>
      <div class="map-sub-line"></div>
      <div class="map-sub-btn">
        <div class="map-sub-btn-in " @click="bindTab('../paint/main')">
          <img src="../../assets/icon-map-pic.png" alt="" class="btn-audio" >
        </div>
      </div>
    </div>
    <div class="map-tab">
      <div class="map-tab-item icon-list" @click="bindTab('../index/main')">
        <img src="../../assets/icon-index-list.png" alt="">
      </div>
      <div class="map-tab-item icon-map" @click="bindTab('../map/main')">
        <img src="../../assets/icon-index-map.png" alt="">
      </div>
      <div class="map-tab-item icon-scan" @click="bindTab('../scan/main')">
        <img src="../../assets/icon-index-scan.png" alt="">
      </div>
      <div class="map-tab-item icon-quiz" @click="bindTab('../quiz/main')">
        <img src="../../assets/icon-index-quiz.png" alt="">
      </div>
      <div class="map-tab-item icon-my" @click="bindTab('../my/main')">
        <img src="../../assets/icon-index-my.png" alt="">
      </div>
    </div>
    <scroll-view scroll-x scroll-y class="index-bg">
      <img src="https://gw.alicdn.com/tfs/TB1Ov4FhwHqK1RjSZFkXXX.WFXa-3000-4641.jpg" alt="" >
    </scroll-view>
  </div>
</template>

<script>
export default {
  data() {
    return {
      
    };
  },

  components: {},

  methods: {
    bindTab(url) {
      wx.navigateTo({ url: url });
    },
    locate() {

    }
  },

  created() {

  }
};
</script>

<style scoped lang="less">
.center {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.container {
  position: relative;
}
.map-tab {
  height: 150rpx;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
  display: flex;
  justify-content: space-around;
  align-items: center;
  background: #d5aa80;
  z-index: 99;
  &-item {
    width: 100rpx;
    height: 110rpx;
    img {
      width: 100%;
      height: 100%;
      display: block;
    }
  }
}
.index-bg {
  width: auto;
  height: auto;
  padding-bottom:138rpx;
  // display: block;
  img{
    width: 1000rpx;
    height: 1640rpx;
  }
}
.map-sub {
  position: fixed;
  top: 0;
  right: 25rpx;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  z-index: 66;
  &-line {
    width: 2px;
    height: 38rpx;
    background: #9e7044;
  }
  &-btn {
    width: 66rpx;
    height: 66rpx;
    border-radius: 50%;
    border: 2rpx solid #9e7044;
    background: #f6ca47;
    .center();
    &-in {
      width: 50rpx;
      height: 50rpx;
      border-radius: 50%;
      border: 2rpx solid #9e7044;
      background: #f7eec5;
      position: relative;
      .center();
    }
  }
  
  .btn-show {
    width: 30rpx;
    height: 30rpx;
  }
  .btn-audio {
    width: 34rpx;
    height: 28rpx;
  }
}
</style>
