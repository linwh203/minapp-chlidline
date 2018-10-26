<template>
  <div class="container">
    <open-data type="userNickName" class="userName"></open-data>
    <div class="my-tab">
      <img src="https://gw.alicdn.com/tfs/TB1Krl6k3HqK1RjSZFgXXa7JXXa-111-76.png" class="icon-stage">
      <img src="https://gw.alicdn.com/tfs/TB1Ww4Yk9rqK1RjSZK9XXXyypXa-110-72.png" class="icon-reward">
      <img src="https://gw.alicdn.com/tfs/TB1ir86k3HqK1RjSZFgXXa7JXXa-110-80.png" class="icon-clear">
      <img src="https://gw.alicdn.com/tfs/TB1wtV2kY2pK1RjSZFsXXaNlXXa-110-80.png" class="icon-noclear">
    </div>
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
    }
  },

  created() {
    
  }
};
</script>

<style scoped lang="less">
.center{
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}
.container {
  width: 100%;
  height: 100%;
  position: relative;
  background: url('https://gw.alicdn.com/tfs/TB1_p0GkZfpK1RjSZFOXXa6nFXa-640-1008.png') no-repeat top/cover;
  .center();
}
.userName{
  padding:0 60rpx;
  line-height: 70rpx;
  background: #ff9899;
  color:#fff;
  border-radius: 38rpx;
  position: absolute;
  top: 24%;
}
.my-tab{
  position: absolute;
  top: 40%;
  width: 100%;
  &-body{
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 0 105rpx;
    &-item{
      .center();
      width: 210rpx;
      margin-bottom: 36rpx;
      &-icon{
        width: 210rpx;
        height: 156rpx;
        display: block;
      }
      &-name{
        font-size:26rpx;
        color:#3f2306;
      }
    }
  }
}
.icon-stage{
  width: 130rpx;
  height: 88rpx;
}
.icon-reward{
  width: 128rpx;
  height: 84rpx;
}
.icon-clear{
  width: 128rpx;
  height: 94rpx;
}
.icon-noclear{
  width: 128rpx;
  height: 94rpx;
}
</style>
