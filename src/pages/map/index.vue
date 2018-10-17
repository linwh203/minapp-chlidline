<template>
  <movable-area class="container">
    <movable-view class="index-bg" direction="all" :x="x" :y="y" :class="init?'init':''" @change="startTouch">
      <img class="mapImg" src="../../assets/bg-map.png" alt="" >
      <img class="userIcon" id="usericon" src="../../assets/icon-avator.png" v-bind:style="{ top: userLat + 'rpx',left: userLng + 'rpx'}">
    </movable-view>
    <div class="map-sub">
      <div class="map-sub-line"></div>
      <div class="map-sub-btn">
        <div class="map-sub-btn-in" @click="locate">
          <img src="../../assets/icon-map-gps.png" alt="" class="btn-show">
        </div>
      </div>
      <div class="map-sub-line"></div>
      <div class="map-sub-btn">
        <div class="map-sub-btn-in " @click="viewDetail">
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
    <!-- <scroll-view scroll-y scroll-x scroll-into-view="mid" class="index-bg"> -->
    <!-- <scroll-view scroll-x scroll-y scroll-top="1400" scroll-left="210" class="index-bg">
      <img class="mapImg" src="../../assets/bg-map.png" alt="" >
      <img class="userIcon" id="usericon" src="../../assets/icon-avator.png" v-bind:style="{ top: userLat + 'rpx',left: userLng + 'rpx'}">
      <div id="mid"></div>
    </scroll-view> -->
  </movable-area>
</template>

<script>
export default {
  data() {
    return {
      userLng:600,
      userLat:1000,
      activeSpot:'',
      init:true,
      x:0,
      y:0
    };
  },

  components: {},

  methods: {
    bindTab(url) {
      wx.navigateTo({ url: url });
    },
    viewDetail() {

    },
    locate() {

    },
    startTouch() {
      this.init = false
    }
  },

  created() {
    wx.getLocation({
      type: 'wgs84', //默认为 wgs84 返回 gps 坐标，gcj02 返回可用于wx.openLocation的坐标,
      success: res => {
        console.info("getLocation success: ", res);
        const latitude = res.latitude
        const longitude = res.longitude
        const speed = res.speed
        const accuracy = res.accuracy

        this.userLng = 600
        this.userLat = 1000
      },
      fail: () => {
        console.log("getLocation failed")
      }
    });
  },
  onReady() {
    this.x = -200
    this.y = -300
    setTimeout(()=>{
      this.init = false
    },3000)
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
#mid{
  width: 10px;
  height: 10px;
  background: red;
  position: absolute;
  top: 1000rpx;
  left:430rpx;
  z-index: 99999;
}
.container {
  width: 100%;
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
  background: #d1a77f;
  position: relative;
  .mapImg{
    // width: 1560rpx;
    // height: 2640rpx;
    width: 1180rpx;
    height: 2000rpx;
  }
  .userIcon{
    width: 80rpx;
    height: 120rpx;
    position:absolute;
  }
}
.init{
  // transform: translateX(-216px) translateY(-301px) !important;
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
