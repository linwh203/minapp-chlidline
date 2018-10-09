<template>
  <div class="container">
    <div class="index-tab" style="color:#fff;font-size:24rpx;">
      This is My
    </div>
    <img src="../../assets/bg-index.jpg" alt="" class="index-bg">
  </div>
</template>

<script>
export default {
  data() {
    return {
      motto: ""
    };
  },

  components: {},

  methods: {
    bindTab(e) {
      console.log(e.currentTarget);
    }
  },

  created() {
    
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
</style>
