<template>
  <div class="container">
    <div class="sub-nav">
      <div class="sub-nav-line"></div>
      <div class="sub-nav-btn">
        <div class="sub-nav-btn-in" @click="displaySub">
          <img
            src="../../assets/icon-list-show.png"
            alt
            class="btn-show"
            :class="showSub?'':'reverse'"
          >
        </div>
      </div>
      <div class="sub-nav-line" v-if="showSub && audioUrl "></div>
      <div class="sub-nav-btn" v-if="showSub && audioUrl ">
        <div class="sub-nav-btn-in" @click="playAudio">
          <img src="../../assets/icon-list-audio.png" alt class="btn-audio" v-if="audioOff">
          <img src="../../assets/icon-list-audio-play.png" alt class="btn-audio" v-else>
        </div>
      </div>
      <div class="sub-nav-line" v-if="showSub && videoUrl "></div>
      <div class="sub-nav-btn" v-if="showSub && videoUrl ">
        <div class="sub-nav-btn-in" @click="goVideo">
          <img src="../../assets/icon-list-video.png" alt class="btn-video">
        </div>
      </div>
      <div class="sub-nav-line" v-if="showSub"></div>
      <div class="sub-nav-btn" v-if="showSub">
        <div class="sub-nav-btn-in">
          <button open-type="share" class="btn-share-origin"></button>
          <img src="../../assets/icon-list-share.png" alt class="btn-share">
        </div>
      </div>
    </div>
    <div class="article">
      <scroll-view scroll-y class="article-view" :scroll-top="scrollTop">
        <img mode="widthFix" :src="mainPic">
      </scroll-view>
    </div>
    <div class="article-border"></div>
    <div class="index-list">
      <div class="index-list-title">课程目录</div>
      <scroll-view scroll-x>
        <div class="index-list-box">
          <div class="index-list-item" v-for="(item,index) in listItem" :key="index">
            <div
              class="index-list-item-img"
              :class="activeIndex == index+1 ? 'index-list-item-img-active':''"
              @click="changeArticle(index+1)"
            >
              <img :src="'../../assets/list-pic-'+(index+1)+'.png'" alt>
            </div>
            <div class="index-list-item-title">{{item.spot_name}}</div>
          </div>
        </div>
      </scroll-view>
    </div>
    <!-- <div class="index-list-close">
      <div class="index-list-close-body" @click="bindTab">
        <img src="../../assets/btn-close-list.png" alt="">
      </div>
    </div>-->
    <div class="share-box" v-if="sharebox">
      <div class="share-box-body">
        <div class="share-box-body-item">
          <button open-type="share" class="btn-share-origin"></button>
          <img src="../../assets/icon-share-weixin.png" alt>
        </div>
      </div>
      <div class="share-box-close" @click="hideShareBox">取消</div>
    </div>
    <img
      src="https://etx.funsomestudio.com/File/Download?fileName=local/bg-index.jpg&fileType=ChildLineFile"
      alt
      class="index-bg"
    >
  </div>
</template>

<script>
import { config } from "../../utils/index";
export default {
  data() {
    return {
      fromMap: false,
      showSub: true,
      innerAudioContext: null,
      videoUrl: "",
      audioOff: true,
      audioUrl: "",
      mainPic: "",
      scrollTop: 0,
      activeIndex: 1,
      sharebox: false,
      listItem: []
    };
  },
  computed: {},
  components: {},

  methods: {
    setStorage(key, val) {
      try {
        wx.setStorageSync(key, val);
      } catch (e) {
        wx.setStorage(key, val);
      }
    },
    getStorage(key) {
      try {
        wx.getStorageSync(key);
      } catch (e) {
        wx.getStorage(key);
      }
    },
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
      this.sharebox = true;
    },
    hideShareBox() {
      this.sharebox = false;
    },
    changeArticle(id) {
      this.audioOff = true;
      if (this.innerAudioContext) {
        this.innerAudioContext.stop();
      }
      let spot_id = "";
      for (let i = 0; i < this.listItem.length; i++) {
        if (this.listItem[i].sortNo == id) {
          spot_id = this.listItem[i].spot_id;
        }
      }
      const currentId = parseInt(id);
      this.activeIndex = currentId;
      wx.request({
        url: config.base + "spot/listdetail", //开发者服务器接口地址",
        data: {
          spot_id: spot_id,
          lineId: config.lineId
        }, //请求的参数",
        method: "GET",
        dataType: "json", //如果设为json，会尝试对返回的数据做一次 JSON.parse
        success: res => {
          console.log(res.data.data);
          this.mainPic = res.data.data[0].image_url;
          this.innerAudioContext = wx.createInnerAudioContext();
          this.audioUrl = res.data.data[0].audio_url;
          this.videoUrl = res.data.data[0].video_url;
          if (this.audioUrl) {
            this.innerAudioContext.src = this.audioUrl;
          }
        },
        fail: () => {},
        complete: () => {}
      });
    },
    getSpot() {
      const self = this;
      wx.request({
        url: config.base + "spot/list", //开发者服务器接口地址",
        data: {
          lineId: config.lineId
        }, //请求的参数",
        method: "GET",
        dataType: "json", //如果设为json，会尝试对返回的数据做一次 JSON.parse
        success: res => {
          // console.log(res)
          // self.GLOBAL.spot_list = res.data.data
          this.listItem = res.data.data;
          this.setStorage("spotList", res.data.data);
        },
        fail: () => {},
        complete: () => {}
      });
    }
  },
  created() {},
  mounted() {
    // this.innerAudioContext.src = this.audioUrl;
  },
  onLoad(option) {
    if (wx.getStorageSync("spotList")) {
      this.listItem = wx.getStorageSync("spotList");
    } else {
      this.getSpot();
    }
    if (option.from) {
      this.fromMap = true;
      this.changeArticle(option.spot_index);
    } else if (option.spot_index) {
      this.changeArticle(option.spot_index);
    } else {
      this.changeArticle(1);
    }
    this.innerAudioContext = wx.createInnerAudioContext();
  },
  onHide() {
    this.audioOff = true;
    this.showSub = true;
    this.innerAudioContext.stop();
    console.log("list page hide");
    // this.innerAudioContext = null
  },
  onUnload() {
    this.audioOff = true;
    this.showSub = true;
    this.innerAudioContext.stop();
    // this.innerAudioContext = null
  },
  onShow() {
    // console.log(1)
    // this.changeArticle(1,this.listItem[0].spot_id)
    // this.innerAudioContext = wx.createInnerAudioContext();
  },
  onShareAppMessage(result) {
    let title = "儿童研习径";
    // let path = "/pages/list/main?spot_index=" + this.activeIndex;
    let path =
      "/pages/index/main?share_from=list&spot_index=" + this.activeIndex;
    // let imageUrl = "../../assets/list-pic-1.png";
    // let desc = '这里是描述哦'
    // if (result.from === "button") {
    //   this.billId = "billId-" + new Date().getTime();
    //   title = "我发起了一个转发";
    //   path = `pages/index/main?billId=${this.billId}`;
    // }
    return {
      title,
      path,
      // imageUrl,
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
    @size: 80rpx;
    width: @size;
    height: @size;
    border-radius: 50%;
    border: 2rpx solid #9e7044;
    background: #f6ca47;
    .center();
    &-in {
      @insize: 0.76 * @size;
      width: @insize;
      height: @insize;
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
  height: 84%;
  &-view {
    height: 100%;
    background-color: #c4ebfc;
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
    top: 85%;
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
  &-title {
    margin: 10rpx 0 10rpx 20rpx;
    color: white;
    height: 20rpx;
    font-size: 34rpx;
  }
  img {
    width: 100%;
    height: 100%;
    display: block;
  }
  &-box {
    padding-top: 5%;
    height: 20%;
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-items: center;
    padding-left: 15rpx;
  }
  &-item {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-right: 20rpx;
    padding-bottom: 10rpx;

    &-img {
      @size: 140rpx;
      width: @size;
      height: @size;
      border: 2px solid #b28e69;
      border-radius: 50%;
      background: #fff;
      margin-bottom: 12rpx;
    }
    &-img-active {
      border: 2px solid #00cbff;
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
    bottom: 16.5%;
    right: 74rpx;
    z-index: 9999;
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
.share-box {
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
  &-body {
    height: 220rpx;
    .center();
    &-item {
      position: relative;
      img {
        width: 144rpx;
        height: 168rpx;
        display: block;
      }
    }
  }
  &-close {
    width: 100%;
    height: 100rpx;
    background: #efefef;
    color: #000;
    font-size: 34rpx;
    text-align: center;
    line-height: 98rpx;
  }
}
.btn-share-origin {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: transparent;
  border: none;
}
.btn-share-origin::after {
  border: 0;
}
</style>
