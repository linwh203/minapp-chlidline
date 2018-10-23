<template>
  <div class="container">
    <div class="quiz-top">
      <div class="quiz-top-left" @click="prev"></div>
      <div class="quiz-top-mid">{{index+1}}</div>
      <div class="quiz-top-right" @click="next"></div>
    </div>
    <div class="quiz-mid">
      <div class="quiz-mid-question">
        <div class="quiz-mid-question-body">
          <div class="quiz-mid-question-body-text">{{currentQuiz.quiz}}</div>
          <img class="quiz-mid-question-body-play" src="https://gw.alicdn.com/tfs/TB1.aY2j9zqK1RjSZPxXXc4tVXa-69-70.png" v-if="hasAudio && !isPlayAudio" />
          <img class="quiz-mid-question-body-pause" src="https://gw.alicdn.com/tfs/TB163v1j9rqK1RjSZK9XXXyypXa-69-70.png" v-if="hasAudio && isPlayAudio" />
        </div>
      </div>
      <div class="quiz-mid-msg">从下面选出正确的答案</div>
      <div class="quiz-mid-hint" @click="lookHint">
        <span></span> 提示
      </div>
    </div>
    <div class="quiz-choice">
      <div class="quiz-choice-body">
        <div class="quiz-choice-body-item" @click="chooseItem(0,currentQuiz.answer_list[0])">
          <span class="quiz-choice-body-item-text"><span class="quiz-choice-body-item-icon" v-if="choiceIndex == 0"></span>{{currentQuiz.answer_list[0]}}</span> 
        </div>
        <div class="quiz-choice-body-item" @click="chooseItem(1,currentQuiz.answer_list[1])">
          <span class="quiz-choice-body-item-text"><span class="quiz-choice-body-item-icon" v-if="choiceIndex == 1"></span>{{currentQuiz.answer_list[1]}}</span> 
        </div>
        <div class="quiz-choice-body-item" @click="chooseItem(2,currentQuiz.answer_list[2])">
          <span class="quiz-choice-body-item-text"><span class="quiz-choice-body-item-icon" v-if="choiceIndex == 2"></span>{{currentQuiz.answer_list[2]}}</span> 
        </div>
        <div class="quiz-choice-body-item" @click="chooseItem(3,currentQuiz.answer_list[3])">
          <span class="quiz-choice-body-item-text"><span class="quiz-choice-body-item-icon" v-if="choiceIndex == 3"></span>{{currentQuiz.answer_list[3]}}</span> 
        </div>
      </div>
    </div>
    <div class="quiz-btm" @click="submitAnswer">

    </div>
    <div class="modal" v-if="showHint || showAnswer">
      <div class="modal-container">
        <div class="hint" v-if="showHint">
          <div class="hint-title">温馨提示</div>
          <div class="hint-text">
            {{currentQuiz.tooltip}}
          </div>
          <div class="hint-close" @click="closeHint">
            <img src="../../assets/btn-close-list.png" alt="">
          </div>
        </div>
        <div class="hint answer" v-if="showAnswer">
          <div class="answer-title">{{wrongAnswer?'再想想':'你真棒'}}</div>
          <div class="answer-text">
            {{wrongAnswer?'不是'+choiceName+'选错了!':'找对了,继续加油哦!'}}
          </div>
          <div class="answer-btn" @click=closeAnswer>
            {{wrongAnswer?'重新回答':'下一题'}}
          </div>
          <div class="hint-close" @click="closeAnswer">
            <img src="../../assets/btn-close-list.png" alt="">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      index: 0,
      showHint:false,
      hasAudio:false,
      isPlayAudio:false,
      choiceIndex:-1,
      choiceName:'',
      currentQuiz:{
        id:null,title:null,type:null,tooltip:null,quiz:null,answer_list:[],right_answer:null,is_right:null
      },
      wrongAnswer:false,
      showAnswer:false,
      questionList:[
        {
          // 题目id
          id: 1,
          // 标题
          title: 'this is title No.1',
          // 题目类型,类型枚举:'文本选择 text','看图识别 image','声音识别 video',
          type: 1,
          // 问题的提示
          tooltip: 'this is hint No.1',
          // 问题正文,如果是'看图识别'和'声音识别'就应该是个url字符串
          quiz: 'this is the question No.1',
          // 答案列表,如果是'看图识别'就应该是个图片地址
          answer_list: ['A choice 1','B choice 2','C choice 3','D choice 4'],
          // 正确答案的序号
          right_answer: 3,
          // 我是否答对
          is_right: false
        },
        {id:2,title:'this is title No.2',type:2,tooltip:'this is hint No.2',quiz:'this is the question No.2',
          answer_list:['A choice 11','B choice 22','C choice 33','D choice 44'],right_answer:2,is_right:false},
        {id:3,title:'this is title No.3',type:3,tooltip:'this is hint No.3',quiz:'this is the question No.3',
          answer_list:['A choice 111','B choice 222','C choice 333','D choice 444'],right_answer:3,is_right:false},
      ]
      
    };
  },

  components: {},

  methods: {
    bindTab(e) {
      wx.navigateTo({ url: "../index/main" });
    },
    chooseItem(index, name) {
      this.choiceIndex = index
      this.choiceName = name
    },
    submitAnswer() {
      this.showAnswer = true
      if (this.choiceIndex != this.currentQuiz.right_answer) {
        this.wrongAnswer = true
      }
    },
    lookHint() {
      this.showHint = true
    },
    closeHint() {
      this.showHint = false
    },
    closeAnswer() {
      this.showAnswer = false
      if (this.wrongAnswer) {
        this.wrongAnswer = false
      } else {
        this.questionList[this.index].is_right = true
        this.index += 1
        this.choiceIndex = -1
        this.choiceName = ''
        this.currentQuiz = this.questionList[this.index]
      }
    },
    prev() {
      if (this.index == 0) {
        return
      } else {
        this.index --
        this.currentQuiz = this.questionList[this.index]
        this.choiceIndex = this.currentQuiz.right_answer
      }
    },
    next() {
      if (this.index == this.questionList.length-1) {
        return 
      } else {
        if (this.questionList[this.index].is_right) {
          this.index ++
          this.currentQuiz = this.questionList[this.index]
          this.questionList[this.index].is_right?this.choiceIndex = this.currentQuiz.right_answer:this.choiceIndex = -1
        } else {
          return
        }
      }
    }
  },

  created() {
    this.currentQuiz = this.questionList[0]
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
  background: url('https://gw.alicdn.com/tfs/TB1gKgDjSzqK1RjSZFjXXblCFXa-640-1008.jpg') no-repeat top/100% 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  color:#9e7044;
}
.quiz-top{
  width: 100%;
  display: flex;
  justify-content: space-between;
  margin-top:62rpx;
  &-left{
    width: 80rpx;
    height: 80rpx;
    background: url('https://gw.alicdn.com/tfs/TB1pKAAjPTpK1RjSZKPXXa3UpXa-72-74.png') no-repeat top/100% 100%;
    margin-left: 39rpx;
  }
  &-mid{
    width: 110rpx;
    height: 110rpx;
    line-height: 110rpx;
    text-align: center;
    font-size: 56rpx;
    background: url('https://gw.alicdn.com/tfs/TB1Cd3zjNTpK1RjSZFGXXcHqFXa-90-90.png') no-repeat top/100% 100%;
    margin-left: 14rpx;
    margin-top:6rpx;
  }
  &-right{
    width: 80rpx;
    height: 80rpx;
    background: url('https://gw.alicdn.com/tfs/TB1.CowjSzqK1RjSZPxXXc4tVXa-72-74.png') no-repeat top/100% 100%;
    margin-right: 65rpx;
  }
}
.quiz-mid{
  position: absolute;
  top: 180rpx;
  left: 80rpx;
  &-question{
    width: 545rpx;
    height: 180rpx;
    padding:50rpx 20rpx 20rpx;
    background: url('https://gw.alicdn.com/tfs/TB1_A.FjQPoK1RjSZKbXXX1IXXa-506-206.png') no-repeat top/100% 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    &-body{
      font-size: 30rpx;
      text-align: center;
      &-play{
        margin-top: 20rpx;
        width: 82rpx;
        height: 82rpx;
      }
      &-pause{
        margin-top: 20rpx;
        width: 82rpx;
        height: 82rpx;
      }
    }
   
  }
  &-hint{
    width: 105rpx;
    height: 48rpx;
    position: absolute;
    background: #2f83d0;
    right: -63rpx;
    bottom: -64rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    color:#fff;
    border-top-left-radius: 20rpx;
    border-bottom-left-radius: 20rpx;
    font-size:24rpx;
    span{
      display: block;
      width: 26rpx;
      height: 35rpx;
      margin-right: 12rpx;
      background: url('https://gw.alicdn.com/tfs/TB1rGEDjPTpK1RjSZKPXXa3UpXa-26-35.png') no-repeat top/cover;
    }
  }
  &-msg{
    position: absolute;
    bottom: -56rpx;
    left: 0;
    right: 0;
    margin:auto;
    text-align: center;
    font-size:24rpx;
  }
}
.quiz-choice{
  width: 540rpx;
  height: 326rpx;
  padding:62rpx 44rpx;
  background: url('https://gw.alicdn.com/tfs/TB1rmQyjNTpK1RjSZR0XXbEwXXa-536-384.png') no-repeat top/cover;
  margin-top:310rpx;
  &-body{
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    &-item{
      width: 100%;
      height: 25%;
      display: flex;
      justify-content: center;
      align-items: center;
      border-bottom:1px solid #9e7044;
      &-text{
        position: relative;
      }
      &-icon{
        width: 46rpx;
        height: 38rpx;
        display: block;
        background: url('https://gw.alicdn.com/tfs/TB1_3MDj9zqK1RjSZFHXXb3CpXa-46-38.png') no-repeat top/cover;
        position:absolute;
        left: -50rpx;
      }
    }
    &-item:nth-last-of-type(1){
      border-bottom:none;
    }
  }
}
.quiz-btm{
  width: 348rpx;
  height: 118rpx;
  background: url('https://gw.alicdn.com/tfs/TB1KWMEjIfpK1RjSZFOXXa6nFXa-300-98.png') no-repeat top/cover;
  margin-bottom: 100rpx;
}
.modal {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  &-container {
    position: absolute;
    top: 548rpx;
    width: 540rpx;
    height: 336rpx;
    background: #fff;
    border-radius: 10px;
    overflow: hidden;
  }
}
.hint{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  height: 100%;
  position: relative;
  &-title{
    color:#fff;
    text-align: center;
    height: 90rpx;
    line-height: 90rpx;
    background: #53c6f7;
    width: 100%;
  }
  &-text{
    color:#000;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    flex-direction: column;
    height: 100%;
  }
  &-close{
    height: 54rpx;
    width: 52rpx;
    position: absolute;
    top: 0;
    right: 0;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    border-bottom-left-radius: 25rpx;
    img{
      width: 32rpx;
      height: 32rpx;
      display: block;
    }
  }
}
.answer{
  color:#000;
  width: 100%;
  &-title{
    font-size: 40rpx;
    line-height: 130rpx;
  }
  &-text{
    width: 100%;
    font-size: 30rpx;
    text-align: center;
    padding-bottom: 60rpx;
    border-bottom:1px solid #a0a0a0;
  }
  &-btn{
    color:#00baea;
    font-size: 36rpx;
    line-height: 90rpx;
  }
}
</style>
