<template>
  <div class="container">
    <scroll-view scroll-y class="article-view">
      <div class="msg-item">
        <div class="msg-item-pic"></div>
        <div class="msg-item-main">
          <div class="msg-item-main-top">
            <div class="msg-item-main-top-left">这里是标题</div>
            <div class="msg-item-main-top-right">这里是时间</div>
          </div>
          <div class="msg-item-main-btm">
            这里是正文。这里是正文。这里是正文。这里是正文。这里是正文。这里是正文。这里是正文。
          </div>
        </div>
      </div>
    </scroll-view>
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

  },

  created() {},
  mounted() {
  }
};
</script>

<style scoped lang="less">
.container{
  background: #fff;
}
.article-view{
  width: 100%;
}
img{
  width: 100%;
}
.msg-item{
  width: 100%;
  margin-top: 26rpx;
  // height: 160rpx;
  display: flex;
  &-pic{
    width: 116rpx;
    height: 116rpx;
    border-radius: 50%;
    border:1px solid #e0e0e0;
    overflow: hidden;
    background: #e0e0e0;
    margin-left: 23rpx;
    margin-right: 15rpx;
  }
  &-main{
    width: 80%;
    border-bottom:1px solid #e0e0e0;
    &-top{
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-right:25rpx;
      &-left{
        font-size: 34rpx;
      }
      &-right{
        font-size: 24rpx;
        color:#909090;
      }
    }
    &-btm{
      margin-right:25rpx;
      margin-bottom:24rpx;
      font-size: 24rpx;
      color:#909090;
    }
  }
}
.ellipsis{
  overflow:hidden;
  white-space:nowrap;
  text-overflow:ellipsis;
}
</style>
