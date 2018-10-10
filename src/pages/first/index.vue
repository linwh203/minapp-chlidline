<template>
  <div class="container">
    <div class="first-visit">
      <img src="../../assets/bg-first-visit.jpg" alt="">
      <div class="close-btn" @click="bindNext"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },

  components: {},

  methods: {
    bindNext() {
      wx.setStorageSync('firsttime', true)
      const url = "../index/main";
      wx.redirectTo({ url: url });
    }
  },

  created() {
    
  }
};
</script>

<style scoped lang="less">
// rate width 1.171,height 1.174
@rate: 1.17;
.first-visit {
  width: 100%;
  height: 100%;
  position: relative;
  font-size: 0;
  image {
    width: 100%;
    height: 100%;
    display: block;
  }
}
.close-btn {
  width: 46rpx;
  height: 46rpx;
  position: absolute;
  right: 29rpx;
  top: 29rpx;
  background: url("./../../assets/btn-close-1.png") no-repeat center/cover;
}
</style>
