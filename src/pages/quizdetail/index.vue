<template>
  <div class="container">
    <div class="quiz-top">
      <div class="quiz-top-left"></div>
      <div class="quiz-top-mid">{{index}}</div>
      <div class="quiz-top-right"></div>
    </div>
    <div class="quiz-mid"></div>
    <div class="quiz-btm"></div>
    
  </div>
</template>

<script>
export default {
  data() {
    return {
      index: 1
    };
  },

  components: {},

  methods: {
    bindTab(e) {
      wx.navigateTo({ url: "../index/main" });
    },
  },

  created() {
  },
  onLoad(option) {
    console.log(option.quizid)
  },
};
</script>

<style scoped lang="less">
.container {
  position: relative;
  width: 100%;
  height: 100%;
  background: url('https://gw.alicdn.com/tfs/TB1wy.mi7voK1RjSZFDXXXY3pXa-640-1006.jpg') no-repeat top/cover;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  color:#9e7044;
}
.quiz-top{
  width: 100%;
  display: flex;
  justify-content: space-between;
  margin-top:62rpx;
  &-left{
    width: 60rpx;
    height: 60rpx;
    background: red;
    margin-left: 39rpx;
  }
  &-mid{
    width: 90rpx;
    height: 90rpx;
    line-height: 90rpx;
    text-align: center;
    font-size: 56rpx;
    background: yellow;
    margin-left: 14rpx;
    margin-top:6rpx;
  }
  &-right{
    width: 60rpx;
    height: 60rpx;
    background: blue;
    margin-right: 65rpx;
  }
}
.quiz-mid{

}
.quiz-btm{

}


</style>
