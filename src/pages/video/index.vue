<template>
  <div class="container">
    <div class="index-tab" style="color:#fff;font-size:24rpx;">
      This is video
    </div>
    <img src="../../assets/bg-index.jpg" alt="" class="index-bg">
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
.index-bg {
  width: 100%;
  height: 100%;
  display: block;
}
</style>
