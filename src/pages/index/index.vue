<template>
  <div class="container">
    <div class="index-tab">
      <div class="index-tab-item icon-list" @click="bindTab('../list/main')">
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
    <div class="modal" v-if="motto">
      <div class="modal-container">
        <button open-type="getUserInfo" @getuserinfo="bindGetUserInfo" @click="getUserInfo1">授权登录</button>
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
    getUserInfo() {
      var that = this
      // 查看是否授权
      wx.getSetting({
        success(res) {
          console.log('success')
          if (res.authSetting["scope.userInfo"]) {
            // 已经授权，可以直接调用 getUserInfo 获取头像昵称
            wx.getUserInfo({
              success: function(res) {
                console.log(res.userInfo);
              }
            });
          } else {
            that.motto = true
          }
        },
        fail(res) {
          console.log('fail')
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
    bindTab(url) {
      wx.navigateTo({ url: url });
    },
    getUserInfo1(){
      console.log('click事件首先触发')
      // 判断小程序的API，回调，参数，组件等是否在当前版本可用。  为false 提醒用户升级微信版本
      // console.log(wx.canIUse('button.open-type.getUserInfo'))
      if(wx.canIUse('button.open-type.getUserInfo')){
        // 用户版本可用
      }else{
        console.log('请升级微信版本')
      }
    },
    bindGetUserInfo(e) {
      console.log(e.mp.detail)
      if (e.mp.detail.rawData){
        //用户按了允许授权按钮
        console.log('用户按了允许授权按钮')
      } else {
        //用户按了拒绝按钮
        console.log('用户按了拒绝按钮')
      }
    }
  },

  created() {
    // 调用应用实例的方法获取全局数据
    this.getUserInfo();
  },
  mounted() {
    
  },
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
  justify-content: space-around;
  align-items: center;
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
  width: 100%;
  height: 100%;
  display: block;
}
.modal {
  width: 100%;
  height: 100%;
  position: fixed;
  background: rgba(0,0,0,.3);
}
</style>
