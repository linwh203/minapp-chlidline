<template>
  <div class="container">
    <video id="myVideo" :src="videoSrc" :poster="coverSrc" controls objectFit="cover" class="video-container" v-if="!showModal"></video>
    <div class="modal" v-if="showModal">
      <div class="modal-top"><img src="../../assets/video-hint-1.png" alt=""></div>
      <div class="modal-mid"><img src="../../assets/video-hint-2.png" alt=""></div>
      <div class="modal-btm" @click="closeModal"><img src="../../assets/video-hint-3.png" alt=""></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showModal: false,
      coverSrc: 'https://gw.alicdn.com/tfs/TB1IegWhkPoK1RjSZKbXXX1IXXa-640-1008.jpg',
      videoSrc: 'http://wxsnsdy.tc.qq.com/105/20210/snsdyvideodownload?filekey=30280201010421301f0201690402534804102ca905ce620b1241b726bc41dcff44e00204012882540400&bizid=1023&hy=SH&fileparam=302c020101042530230204136ffd93020457e3c4ff02024ef202031e8d7f02030f42400204045a320a0201000400'
    };
  },

  components: {},

  methods: {
    closeModal() {
      wx.setStorageSync('firstvideo', true)
      this.showModal = false
    }
  },

  created() {
    const isFirst = wx.getStorageSync("firstvideo");
    if (!isFirst) {
      this.showModal = true
    }
  }
};
</script>

<style scoped lang="less">
.container {
  position: relative;
}
.video-container {
  width: 100%;
  height: 100%;
  display: block;
}
.modal{
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  // background: rgba(0,0,0,.6);
  background: url('https://gw.alicdn.com/tfs/TB1IegWhkPoK1RjSZKbXXX1IXXa-640-1008.jpg') no-repeat top/cover;
  z-index:999;
  &-top{
    position: absolute;
    width: 460rpx;
    height: 180rpx;
    top: 0;
    left: 40rpx;
  }
  &-mid{
    position: absolute;
    width: 452rpx;
    height: 132rpx;
    top: 0;bottom:0;
    left: 0;right: 0;margin: auto;
  }
  &-btm{
    position: absolute;
    width: 300rpx;
    height: 84rpx;
    bottom: 100rpx;
    left: 0;right: 0;margin: auto;
  }
}
image{
  width: 100%;
  height:100%;
  display: block;
}
</style>
