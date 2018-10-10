<template>
  <div class="container">
    <div class="index-tab">
      <div class="index-tab-item icon-list" @click="showList">
        <img src="../../assets/icon-index-list.png" alt="">
      </div>
      <div class="index-tab-item icon-map" @click="bindTab('../map/main')">
        <img src="../../assets/icon-index-map.png" alt="">
      </div>
      <div class="index-tab-item icon-scan" @click="bindTab('../scan/main')">
        <img src="../../assets/icon-index-scan.png" alt="">
      </div>
      <div class="index-tab-item icon-quiz" @click="bindTab('../quiz/main')">
        <img src="../../assets/icon-index-quiz.png" alt="">
      </div>
      <div class="index-tab-item icon-my" @click="bindTab('../my/main')">
        <img src="../../assets/icon-index-my.png" alt="">
      </div>
    </div>
    <scroll-view scroll-x class="index-list">
      <div class="index-list-box">
        <div class="index-list-item">
          <div class="index-list-item-img"><img src="../../assets/list-pic-1.png" alt=""></div>
          <div class="index-list-item-title">深圳多样的生命</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img"><img src="../../assets/list-pic-2.png" alt=""></div>
          <div class="index-list-item-title">哦,你就是蝴蝶</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img"><img src="../../assets/list-pic-3.png" alt=""></div>
          <div class="index-list-item-title">生命的互联网故事</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img"><img src="../../assets/list-pic-4.png" alt=""></div>
          <div class="index-list-item-title">看花识蔬菜</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img"><img src="../../assets/list-pic-5.png" alt=""></div>
          <div class="index-list-item-title">农科院基地的候鸟</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img"><img src="../../assets/list-pic-6.png" alt=""></div>
          <div class="index-list-item-title">大眼睛飞行侠蜻蜓</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img"><img src="../../assets/list-pic-7.png" alt=""></div>
          <div class="index-list-item-title">外星人--昆虫</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img"><img src="../../assets/list-pic-8.png" alt=""></div>
          <div class="index-list-item-title">如何种植水稻</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img"><img src="../../assets/list-pic-9.png" alt=""></div>
          <div class="index-list-item-title">飞吧，鸟儿们</div>
        </div>
        <div class="index-list-item">
          <div class="index-list-item-img"><img src="../../assets/list-pic-10.png" alt=""></div>
          <div class="index-list-item-title">听一场自然音乐会</div>
        </div>
      </div>
      <div class="index-list-close">
        <div class="index-list-close-body">
          <img src="../../assets/btn-close-list.png" alt="">
        </div>
      </div>
    </scroll-view>
    <div class="modal" v-if="motto">
      <div class="modal-container">
        <img src="../../assets/bg-auth.png" alt="">
        <button class="auth-confirm" open-type="getUserInfo" @getuserinfo="bindGetUserInfo" @click="getUserInfo1">授权登录</button>
        <button class="auth-cancel" @click="cancelAuth">拒绝授权</button>
      </div>
    </div>
    <img src="../../assets/bg-index.jpg" alt="" class="index-bg">
  </div>
</template>

<script>
export default {
  data() {
    return {
      motto: false
    };
  },

  components: {},

  methods: {
    getUser() {
      var that = this;
      // 查看是否授权
      wx.getSetting({
        success(res) {
          console.log("success");
          if (res.authSetting["scope.userInfo"]) {
            // 已经授权，可以直接调用 getUserInfo 获取头像昵称
            wx.getUserInfo({
              success: function(res) {
                console.log(res.userInfo);
              }
            });
          } else {
            that.motto = true;
          }
        },
        fail(res) {
          console.log("fail");
          console.log(res);
        }
      });
      // 调用登录接口
      /* wx.login({
        success: () => {
          wx.getUserInfo({
            success: res => {
              this.userInfo = res.userInfo;
            }
          });
        }
      }); */
    },
    showList() {},
    bindTab(url) {
      wx.navigateTo({ url: url });
    },
    getUserInfo1() {
      console.log("click事件首先触发");
      // 判断小程序的API，回调，参数，组件等是否在当前版本可用。  为false 提醒用户升级微信版本
      // console.log(wx.canIUse('button.open-type.getUserInfo'))
      if (wx.canIUse("button.open-type.getUserInfo")) {
        // 用户版本可用
      } else {
        console.log("请升级微信版本");
      }
    },
    bindGetUserInfo(e) {
      console.log(e.mp.detail);
      if (e.mp.detail.rawData) {
        //用户按了允许授权按钮
        console.log("用户按了允许授权按钮");
        const userInfo = e.mp.detail.userInfo;
        const nickName = userInfo.nickName;
        const avatarUrl = userInfo.avatarUrl;
        const gender = userInfo.gender; //性别 0：未知、1：男、2：女
        const province = userInfo.province;
        const city = userInfo.city;
        const country = userInfo.country;
        wx.setStorageSync("userInfo", userInfo);
        this.motto = false;
      } else {
        //用户按了拒绝按钮
        console.log("用户按了拒绝按钮");
      }
    },
    cancelAuth() {
      this.motto = false;
    }
  },

  created() {
    // 调用应用实例的方法获取全局数据
    this.getUser();
  },
  mounted() {
    console.log(this.GLOBAL);
  },
  onLoad() {
    // 判断是否第一次使用
    const firsttime = wx.getStorageSync("firsttime");
    if (!firsttime) {
      const url = "../first/main";
      wx.redirectTo({ url: url });
    } else {
      console.log(firsttime);
    }
  }
};
</script>

<style scoped lang="less">
.container {
  position: relative;
}
.index-tab {
  position: absolute;
  bottom: 15rpx;
  left: 0;
  right: 0;
  margin: auto;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  &-item {
    width: 100rpx;
    height: 110rpx;
  }
}
.index-list {
  position: absolute;
  bottom: 0;
  width: 100%;
  background: rgba(0, 0, 0, 0.7);
  &-box{
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
      border-radius: 50%;
      background: #fff;
      margin-bottom: 12rpx;
    }
    &-title{
      width: 160rpx;
      color:#fff;
      font-size:20rpx;
      text-align: center;
    }
  }
  &-close{
    width: 80rpx;
    height: 80rpx;
    border-radius: 50%;
    position: fixed;;
    bottom: 180rpx;
    right: 74rpx;
    background: rgba(0, 0, 0, .7);
    display: flex;
    align-items: center;
    justify-content: center;
    &-body{
      width: 60rpx;
      height: 60rpx;
      border-radius: 50%;
      background: rgba(0, 0, 0, 1);
      display: flex;
      align-items: center;
      justify-content: center;
      img{
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
.modal {
  width: 100%;
  height: 100%;
  position: fixed;
  background: rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  &-container {
    width: 544rpx;
    height: 696rpx;
    position: relative;
  }
}
.auth-confirm {
  width: 224rpx;
  height: 92rpx;
  position: absolute;
  right: 42rpx;
  bottom: 0;
  opacity: 0;
}
.auth-cancel {
  width: 224rpx;
  height: 92rpx;
  position: absolute;
  left: 42rpx;
  bottom: 0;
  opacity: 0;
}
img {
  width: 100%;
  height: 100%;
  display: block;
}
</style>
