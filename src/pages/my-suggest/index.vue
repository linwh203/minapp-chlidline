<template>
  <div class="container">
    <div class="section">
      <textarea auto-height placeholder="请输入遇到的问题与建议..." v-model="text"/>
    </div>
    <div class="section flex">
      <img src="../../assets/icon-my-suggest.png" class="upload" @click="upload">
      <div class="upload-text">上传照片</div>
    </div>
    {{text}}
  </div>
</template>

<script>
export default {
  data() {
    return {
      text:''
    };
  },

  components: {},

  methods: {
    upload() {
      wx.chooseImage({
        count: '9', //最多可以选择的图片张数,
        success: res => {}, //返回图片的本地文件路径列表 tempFilePaths,
        fail: () => {},
        complete: () => {}
      });
    }
  },

  created() {},
  mounted() {
  }
};
</script>

<style scoped lang="less">
.container{
  background: #f3f3f3;
}
.section{
  background: #fff;
  padding:20rpx;
  min-height:160rpx;
  font-size:26rpx;
}
.upload{
  width: 140rpx;
  height: 140rpx;
  display: block;
  &-text{
    color:#909090;
    margin-left:20rpx;
  }
}
.flex{
  display: flex;
  justify-content: flex-start;
  align-items: center;
  border-bottom:1px solid #e0e0e0;
}
</style>
