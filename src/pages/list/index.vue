<template>
  <div class="container">
    <div class="sub-nav">
      <div class="sub-nav-line"></div>
      <div class="sub-nav-btn">
        <div class="sub-nav-btn-in" @click="displaySub">
          <img src="../../assets/icon-list-show.png" alt="" class="btn-show" :class="showSub?'':'reverse'">
        </div>
      </div>
      <div class="sub-nav-line" v-if="showSub && audioUrl != ''"></div>
      <div class="sub-nav-btn" v-if="showSub && audioUrl != ''">
        <div class="sub-nav-btn-in " @click="playAudio">
          <img src="../../assets/icon-list-audio.png" alt="" class="btn-audio" v-if="audioOff">
          <img src="../../assets/icon-list-audio-play.png" alt="" class="btn-audio" v-else>
        </div>
      </div>
      <div class="sub-nav-line" v-if="showSub && videoUrl != ''"></div>
      <div class="sub-nav-btn" v-if="showSub && videoUrl != ''">
        <div class="sub-nav-btn-in " @click="goVideo">
          <img src="../../assets/icon-list-video.png" alt="" class="btn-video">
        </div>
      </div>
      <div class="sub-nav-line" v-if="showSub"></div>
      <div class="sub-nav-btn" v-if="showSub">
        <div class="sub-nav-btn-in ">
          <img src="../../assets/icon-list-share.png" alt="" class="btn-share" @click="showShareBox">
        </div>
      </div>
    </div>
    <div class="article">
      <scroll-view scroll-y class="article-view" :scroll-top="scrollTop">
        <img mode="widthFix" :src="mainPic">
      </scroll-view>
    </div>
    <div class="article-border"></div>
    <scroll-view scroll-x class="index-list">
      <div class="index-list-box">
        <div class="index-list-item" v-for="(item,index) in listItem" :key=index>
          <div class="index-list-item-img" :class="activeIndex == index+1 ? 'index-list-item-img-active':''" @click="changeArticle(index+1,item)">
            <img :src="'../../assets/list-pic-'+(index+1)+'.png'" alt="">
          </div>
          <div class="index-list-item-title">{{item.spot_name}}</div>
        </div>
      </div>
      <!-- <div class="index-list-box">
        <div class="index-list-item">
          <div class="index-list-item-img" :class="activeIndex == 1 ? 'index-list-item-img-active':''" @click="changeArticle(1)">
            <img src="../../assets/list-pic-1.png" alt="">
          </div>
          <div class="index-list-item-title">深圳多样的生命</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img" :class="activeIndex == 2 ? 'index-list-item-img-active':''" @click="changeArticle(2)">
            <img src="../../assets/list-pic-2.png" alt="">
          </div>
          <div class="index-list-item-title">哦,你就是蝴蝶</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img" :class="activeIndex == 3 ? 'index-list-item-img-active':''" @click="changeArticle(3)">
            <img src="../../assets/list-pic-3.png" alt="">
          </div>
          <div class="index-list-item-title">生命的互联网故事</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img" :class="activeIndex == 4 ? 'index-list-item-img-active':''" @click="changeArticle(4)">
            <img src="../../assets/list-pic-4.png" alt="">
          </div>
          <div class="index-list-item-title">看花识蔬菜</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img" :class="activeIndex == 5 ? 'index-list-item-img-active':''" @click="changeArticle(5)">
            <img src="../../assets/list-pic-5.png" alt="">
          </div>
          <div class="index-list-item-title">农科院基地的候鸟</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img" :class="activeIndex == 6 ? 'index-list-item-img-active':''" @click="changeArticle(6)">
            <img src="../../assets/list-pic-6.png" alt="">
          </div>
          <div class="index-list-item-title">大眼睛飞行侠蜻蜓</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img" :class="activeIndex == 7 ? 'index-list-item-img-active':''" @click="changeArticle(7)">
            <img src="../../assets/list-pic-7.png" alt="">
          </div>
          <div class="index-list-item-title">外星人--昆虫</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img" :class="activeIndex == 8 ? 'index-list-item-img-active':''" @click="changeArticle(8)">
            <img src="../../assets/list-pic-8.png" alt="">
          </div>
          <div class="index-list-item-title">如何种植水稻</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img" :class="activeIndex == 9 ? 'index-list-item-img-active':''" @click="changeArticle(9)">
            <img src="../../assets/list-pic-9.png" alt="">
          </div>
          <div class="index-list-item-title">飞吧，鸟儿们</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img" :class="activeIndex == 10 ? 'index-list-item-img-active':''" @click="changeArticle(10)">
            <img src="../../assets/list-pic-10.png" alt="">
          </div>
          <div class="index-list-item-title">听一场自然音乐会</div>
        </div>
      </div> -->
    </scroll-view>
    <div class="index-list-close">
      <div class="index-list-close-body" @click="bindTab">
        <img src="../../assets/btn-close-list.png" alt="">
      </div>
    </div>
    <div class="share-box" v-if="sharebox">
      <div class="share-box-body">
        <div class="share-box-body-item">
          <button open-type="share" class="btn-share-origin"></button>
          <img src="../../assets/icon-share-weixin.png" alt="">
          
        </div>
      </div>
      <div class="share-box-close" @click="hideShareBox">取消</div>
    </div>
    <img src="../../assets/bg-index.jpg" alt="" class="index-bg">
  </div>
</template>

<script>
import {config} from '../../utils/index'
export default {
  data() {
    return {
      fromMap: false,
      showSub: false,
      innerAudioContext: null,
      videoUrl:'',
      audioOff: true,
      audioUrl: '',
        // "http://ws.stream.qqmusic.qq.com/M500001VfvsJ21xFqb.mp3?guid=ffffffff82def4af4b12b3cd9337d5e7&uin=346897220&vkey=6292F51E1E384E061FF02C31F716658E5C81F5594D561F2E88B854E81CAAB7806D5E4F103E55D33C16F3FAC506D1AB172DE8600B37E43FAD&fromtag=46",
      mainPic: "",
      scrollTop: 0,
      activeIndex: 1,
      sharebox: false,
      listItem:[]
    };
  },
  computed: {

  },
  components: {},

  methods: {
    bindTab() {
      this.fromMap
        ? wx.navigateTo({ url: "../map/main" })
        : wx.navigateTo({ url: "../index/main" });
      // wx.navigateBack();
    },
    displaySub() {
      this.showSub = !this.showSub;
    },
    playAudio() {
      if (this.audioOff) {
        this.audioOff = false;
        this.innerAudioContext.src = this.audioUrl;
        this.innerAudioContext.play();
      } else {
        this.audioOff = true;
        this.innerAudioContext.stop();
      }
    },
    goVideo() {
      wx.navigateTo({ url: "../video/main?video_url=" + this.videoUrl });
    },
    showShareBox() {
      this.sharebox = true
    },
    hideShareBox() {
      this.sharebox = false
    },
    changeArticle(id,item) {
      this.audioOff = true;
      if (this.innerAudioContext) { this.innerAudioContext.stop() }
      const currentId = parseInt(id)
      this.activeIndex = currentId
      wx.request({
        url: config.base + 'spot/listdetail', //开发者服务器接口地址",
        data: {
          spot_id: item.spot_id,
          lineId: config.lineId
        }, //请求的参数",
        method: 'GET',
        dataType: 'json', //如果设为json，会尝试对返回的数据做一次 JSON.parse
        success: res => {
          console.log(res.data.data[0])
          this.audioUrl = res.data.data[0].audio_url == null ? '' : config.base + res.data.data[0].audio_url
          this.videoUrl = res.data.data[0].video_url == null ? '' : config.base + res.data.data[0].video_url
        },
        fail: () => {},
        complete: () => {}
      });
      // this.mainPic = `http://39.106.120.41:8499/${item.spot_image}`
      switch (currentId) {
        case 1:
          this.mainPic = "https://gw.alicdn.com/tfs/TB1xjt7hmzqK1RjSZFLXXcn2XXa-600-6588.png"
          break
        case 2:
          this.mainPic = "https://gw.alicdn.com/tfs/TB1kqGghgDqK1RjSZSyXXaxEVXa-600-2235.png"
          break
        case 3:
          this.mainPic = "https://gw.alicdn.com/tfs/TB1no5jhgHqK1RjSZFkXXX.WFXa-600-6394.png"
          break
        case 4:
          this.mainPic = "https://gw.alicdn.com/tfs/TB1f.idhb2pK1RjSZFsXXaNlXXa-600-4136.png"
          break
        case 5:
          this.mainPic = "https://gw.alicdn.com/tfs/TB1bHihhmzqK1RjSZFLXXcn2XXa-600-6676.png"
          break
        case 6:
          this.mainPic = "https://gw.alicdn.com/tfs/TB1c4unhhTpK1RjSZFMXXbG_VXa-600-2808.png"
          break
        case 7:
          this.mainPic = "https://gw.alicdn.com/tfs/TB1.SOChhnaK1RjSZFBXXcW7VXa-600-8017.png"
          break
        case 8:
          this.mainPic = "https://gw.alicdn.com/tfs/TB1YzKkhkvoK1RjSZFDXXXY3pXa-600-4388.png"
          break
        case 9:
          this.mainPic = "https://gw.alicdn.com/tfs/TB1ChGjhkvoK1RjSZPfXXXPKFXa-600-4368.png"
          break
        case 10:
          this.mainPic = "https://gw.alicdn.com/tfs/TB1g_GjhgHqK1RjSZFkXXX.WFXa-600-6135.png"
          break
      } 
    }
  },

  created() {},
  mounted() {
    this.innerAudioContext = wx.createInnerAudioContext();
    // this.innerAudioContext.src = this.audioUrl;
  },
  onLoad(option) {
    this.listItem = wx.getStorageSync('spotList');
    if (option.from) {
      this.fromMap = true
    }
    if (option.spot_id) {
      this.changeArticle(option.spot_index, option.spot_id)
    } else {
      this.changeArticle(1,this.listItem[0])
      // this.mainPic =
      //   "https://gw.alicdn.com/tfs/TB1xjt7hmzqK1RjSZFLXXcn2XXa-600-6588.png";
    }
  },
  onHide() {
    this.audioOff = true;
    this.showSub = false;
    this.innerAudioContext.stop();
  },
  onShareAppMessage(result) {
    let title = "儿童研习径";
    let path = "/pages/list/main?spot_id=" + this.activeIndex;
    let imageUrl = "../../assets/list-pic-1.png";
    // let desc = '这里是描述哦'
    // if (result.from === "button") {
    //   this.billId = "billId-" + new Date().getTime();
    //   title = "我发起了一个转发";
    //   path = `pages/index/main?billId=${this.billId}`;
    // }
    return {
      title,
      path,
      imageUrl,
      // desc,
      success: res => {
        console.log("success", res);
      },
      fail(e) {
        console.log(e);
      }
    };
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
.sub-nav {
  position: absolute;
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
    height: 16rpx;
  }
  .reverse {
    transform: rotateX(180deg);
  }
  .btn-audio {
    width: 30rpx;
    height: 26rpx;
  }
  .btn-video {
    width: 28rpx;
    height: 26rpx;
  }
  .btn-share {
    width: 24rpx;
    height: 28rpx;
  }
}
.article {
  position: absolute;
  left: 20rpx;
  right: 20rpx;
  top: 20rpx;
  margin: auto;
  border-radius: 8rpx;
  background: #fff;
  overflow: hidden;
  &-view {
    height: 954rpx;
    img {
      width: 100%;
      height: auto;
      display: block;
    }
  }
  &-border {
    height: 16rpx;
    position: absolute;
    left: 10rpx;
    right: 10rpx;
    top: 972rpx;
    margin: auto;
    border-radius: 8rpx;
    background: #a97b4f;
  }
}
.index-list {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  background: rgba(0, 0, 0, 0.7);
  img {
    width: 100%;
    height: 100%;
    display: block;
  }
  &-box {
    padding-top: 40rpx;
    height: 180rpx;
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-items: center;
  }
  &-item {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    &-img {
      width: 120rpx;
      height: 120rpx;
      border:2px solid #b28e69;
      border-radius: 50%;
      background: #fff;
      margin-bottom: 12rpx;
    }
    &-img-active{
      border:2px solid #00cbff;
    }
    &-title {
      width: 160rpx;
      color: #fff;
      font-size: 20rpx;
      text-align: center;
    }
  }
  &-close {
    width: 80rpx;
    height: 80rpx;
    border-radius: 50%;
    position: fixed;
    bottom: 180rpx;
    right: 74rpx;
    z-index: 999;
    background: rgba(0, 0, 0, 0.7);
    display: flex;
    align-items: center;
    justify-content: center;
    &-body {
      width: 60rpx;
      height: 60rpx;
      border-radius: 50%;
      background: rgba(0, 0, 0, 1);
      display: flex;
      align-items: center;
      justify-content: center;
      img {
        width: 32rpx;
        height: 32rpx;
        display: block;
      }
    }
  }
}
.index-bg {
  width: 100%;
  height: 100%;
  display: block;
}
.share-box{
  width: 100%;
  height: 320rpx;
  background: #fff;
  position: fixed;
  bottom: 0;
  right: 0;
  z-index: 1000;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  &-body{
    height: 220rpx;
    .center();
    &-item{
      position: relative;
      img{
        width: 144rpx;
        height: 168rpx;
        display: block;
      }
    }
  }
  &-close{
    width: 100%;
    height: 100rpx;
    background: #efefef;
    color:#000;
    font-size: 34rpx;
    text-align: center;
    line-height: 98rpx;
  }
}
.btn-share-origin {
  position: absolute;
  top: 0;
  left: 0;
  width: 144rpx;
  height: 168rpx;
  background: transparent;
  border:none;
}
.btn-share-origin::after {
  border: 0;
} 
</style>
