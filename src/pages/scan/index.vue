<template>
  <div class="container">
    <camera :device-position="cameraDirection" flash="off" binderror="error" class="cameraArea" v-if="!showResult"></camera>
    <img class="cameraArea" v-if="showResult" :src="src" />
    <div class="index-tab" style="color:#fff;font-size:24rpx;">
      <div class="index-tab-left">
        <img src="../../assets/icon-scan-hint.png" @click="showDesc = !showDesc">
        <p>使用说明</p>
      </div>
      <div class="index-tab-mid">
        <img src="../../assets/icon-scan-photo.png" @click="takePhoto">
        <p>拍一拍</p>
      </div>
      <div class="index-tab-right">
        <img src="../../assets/icon-scan-turn.png" @click="reverse">
        <p>镜头切换</p>
      </div>
    </div>
    <div class="modal" v-if="showResult" @click="init" >
      
    </div>
    <div class="result-tab" v-if="showResult">
        <img :src="src" class="result-tab-pic active">
      </div>
    <cover-view class="desc" v-if="showDesc">
      <cover-view>拍照识别物种：对准你好奇的物种，</cover-view>
      <cover-view>点击拍一拍吧！</cover-view>
    </cover-view>
  </div>
</template>

<script>
export default {
  data() {
    return {
      src: "",
      cameraDirection:'back',
      showDesc:false,
      showResult:false
    };
  },

  components: {},

  methods: {
    bindTab(e) {
      console.log(e.currentTarget);
    },
    reverse() {
      this.cameraDirection == 'back' ? this.cameraDirection = 'front' : this.cameraDirection = 'back'
    },
    init() {
      this.showResult = false
    },
    takePhoto() {
      this.showDesc = false;
      const ctx = wx.createCameraContext()
      ctx.takePhoto({
        quality: 'high',
        success: (res) => {
          this.src = res.tempImagePath
          this.showResult = true
        }
      })
    },
    error(e) {
      console.log(e.detail)
    }
  },

  created() {
    
  },
  onLoad() {
    const firsttime = wx.getStorageSync("firstPhoto");
    if (!firsttime) {
      this.showDesc = true
    }
  },
  onShow() {
    wx.setStorageSync('firstPhoto',true);
  }
};
</script>

<style scoped lang="less">
.container {
  width: 100%;
  height: 100%;
  position: relative;
}
.index-tab {
  height: 24%;
  background: #fff;
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
  display: flex;
  justify-content: space-around;
  align-items: center;
  p{
    margin-top: 18rpx;
    text-align: center;
    font-size: 24rpx;
  }
  &-left {
    color:#000;
    img{
      width: 94rpx;
      height: 94rpx;
    }
  }
  &-mid {
    color:#000;
    img{
      width: 140rpx;
      height: 140rpx;
    }
  }
  &-right {
    color:#000;
    img{
      width: 94rpx;
      height: 94rpx;
    }
  }
}
img {
  width: 100%;
  height: 100%;
  display: block;
}
.cameraArea{
  width: 100%;
  height: 76%;
}
.desc{
  width: 520rpx;
  height: 126rpx;
  padding: 0 20rpx;
  font-size: 30rpx;
  color:#fff;
  border-radius: 18rpx;
  background: rgba(0,0,0,.5);
  border:1px solid #84988b;
  position: absolute;
  top:62%;
  right: 0;
  left: 0;
  margin:auto;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
}
.modal {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.6);
}
.result-tab{
  position: fixed;
  bottom: 0;
  width: 100%;
  height: 60%;
  background: #fff;
  &-pic{

  }
  &-pic:nth-of-type(1){
    margin-left: 100rpx;
  }
  .active{
    width: 550rpx;
    height: 550rpx;
    border-radius: 50%;
    border:2px solid #f3f3f3;
  }
}
</style>
