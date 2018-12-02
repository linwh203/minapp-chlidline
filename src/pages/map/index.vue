<template>
  <movable-area class="container">
    <movable-view
      class="index-bg"
      direction="all"
      :x="x"
      :y="y"
      scale
      :scale-min="scaleValue"
      scale-max="2"
      :scale-value="scaleValue"
      @change="startTouch"
    >
      <img
        class="mapImg"
        src="https://yxj.forestvisual.com/File/Download?fileName=local/bg-map.png&fileType=ChildLineFile"
        alt
        @click="touchMap"
      >
      <img
        class="userIcon"
        id="usericon"
        src="../../assets/icon-avator.png"
        v-bind:style="{ top: 'calc(' + userLat + '% - 120rpx)' ,left: userLng+'%' }"
      >
      <div class="spot" v-for="(item,index) in spotList" :key="item.sortNo">
        <div
          class="spot-icon"
          :class="'icon-'+item.sortNo"
          v-if="activeSpot == index"
          @click="showWindow(index)"
        >
          <img
            class="active-spot"
            src="https://gw.alicdn.com/tfs/TB1aICmi6DpK1RjSZFrXXa78VXa-30-24.png"
          >
        </div>
        <div
          class="spot-icon"
          :class="'icon-'+item.sortNo"
          v-else
          @click="showWindow(index)"
        >{{item.sortNo}}</div>
        <div
          class="spot-window"
          :class="'window-'+item.sortNo"
          v-if="activeWindow == index"
          @click="viewDetail(item)"
        >
          <div class="spot-window-text">{{item.spot_name}}</div>
          <img class="spot-window-img" :src="prefix + item.spot_image">
        </div>
      </div>
    </movable-view>
    <div class="map-sub">
      <div class="map-sub-line"></div>
      <div class="map-sub-btn">
        <div class="map-sub-btn-in" @click="showNear">
          <img src="../../assets/icon-map-gps.png" alt class="btn-show">
        </div>
      </div>
      <div class="map-sub-line"></div>
      <div class="map-sub-btn">
        <div class="map-sub-btn-in" @click="bindTab('../paint/main')">
          <img src="../../assets/icon-map-pic.png" alt class="btn-audio">
        </div>
      </div>
    </div>
    <div class="map-tab">
      <div class="map-tab-item icon-list" @click="bindTab('../index/main')">
        <img src="../../assets/icon-index-list.png" alt>
      </div>
      <div class="map-tab-item icon-map" @click="bindTab('../map/main')">
        <img src="../../assets/icon-index-map.png" alt>
      </div>
      <div class="map-tab-item icon-scan" @click="bindTab('../scan/main')">
        <img src="../../assets/icon-index-scan.png" alt>
      </div>
      <!-- <div class="map-tab-item icon-quiz" @click="bindTab('../quiz/main')"> -->
      <div class="map-tab-item icon-quiz" @click="bindTab('../developing/main')">
        <img src="../../assets/icon-index-quiz.png" alt>
      </div>
      <div class="map-tab-item icon-my" @click="bindTab('../my/main')">
        <img src="../../assets/icon-index-my.png" alt>
      </div>
    </div>
    <div class="message" v-if="showMessage">
      <!-- <div class="message-close">
        <img src="../../assets/btn-close-list.png" alt="" @click="closeMessage">
      </div>-->
      您当前不在研习径范围内,不能进行定位讲解
    </div>
  </movable-area>
</template>

<script>
import { config } from "../../utils/index";

export default {
  data() {
    return {
      scaleValue: 0.65,
      spotList: [],
      activeSpot: -1,
      activeWindow: -1,
      x: 0,
      y: 0,
      mapStart: {
        lng: 114.49358,
        lat: 22.61034
      },
      mapEnd: {
        lng: 114.50023,
        lat: 22.59959
      },
      nearSpot: 0,
      prefix: config.prefix,
      // showMessage: false
      showMessage: true,
      lng: 0,
      lat: 0,
      // 地理坐标轮询的句柄
      _tForLocation: undefined,
      abc: 0
    };
  },
  computed: {
    // 纬度
    //  mapStart: {
    //     lng: 114.49358,
    //     lat: 22.61034
    //   },
    //   mapEnd: {
    //     lng: 114.50023,
    //     lat: 22.59959
    //   },
    userLat() {
      return this.lat2percent(this.lat);
      //  top
      switch (this.nearSpot) {
        case 0:
          return 720;
        case 1:
          return 700;
        case 2:
          return 550;
        case 3:
          return 400;
        case 4:
          return 400;
        case 5:
          return 800;
        case 6:
          return 1030;
        case 7:
          return 1350;
        case 8:
          return 1600;
        case 9:
          return 1400;
        case 10:
          return 1050;
      }
    },
    userLng() {
      // 经度
      return this.lng2percent(this.lng);

      // left
      switch (this.nearSpot) {
        case 0:
          return 450;
        case 1:
          return 470;
        case 2:
          return 500;
        case 3:
          return 500;
        case 4:
          return 800;
        case 5:
          return 780;
        case 6:
          return 750;
        case 7:
          return 650;
        case 8:
          return 850;
        case 9:
          return 500;
        case 10:
          return 370;
      }
    }
  },

  components: {},

  methods: {
    setStorage(key, val) {
      try {
        wx.setStorageSync(key, val);
      } catch (e) {
        wx.setStorage(key, val);
      }
    },
    getStorage(key) {
      try {
        wx.getStorageSync(key);
      } catch (e) {
        wx.getStorage(key);
      }
    },
    // 插值函数
    _insertFn(x1, y1, x2, y2, x) {
      return (x - x2) * ((y2 - y1) / (x2 - x1)) + y2;
    },
    // 经度转百分比
    lng2percent(value) {
      return this._insertFn(114.498955, 70.5, 114.499207, 73.7, value);
    },
    lat2percent(value) {
      return this._insertFn(22.607683, 22.2, 22.601152, 78.7, value);
    },
    // 输入经度纬度返回
    locate2percent(lng, lat) {},
    showNear() {
      if (this.nearSpot == 0) {
        this.showWindow(this.nearSpot);
      } else {
        this.showWindow(this.nearSpot - 1);
      }
    },
    bindTab(url) {
      wx.navigateTo({ url: url });
    },
    viewDetail(item) {
      console.log(item);
      const spotId = item.spot_id;
      const spot_index = item.sortNo;
      wx.navigateTo({
        url:
          "../list/main?spot_id=" +
          spotId +
          "&spot_index=" +
          spot_index +
          "&from=map"
      });
    },
    showWindow(index) {
      this.activeWindow == index
        ? (this.activeWindow = -1)
        : (this.activeWindow = index);
      this.activeSpot == index
        ? (this.activeSpot = -1)
        : (this.activeSpot = index);
      this.active(this.activeSpot);
      switch (index) {
        case 0:
          this.x = -100;
          this.y = -200;
          break;
        case 1:
          this.x = -100;
          this.y = -200;
          break;
        case 2:
          this.x = -100;
          this.y = -100;
          break;
        case 3:
          this.x = -150;
          this.y = -80;
          break;
        case 4:
          this.x = -100;
          this.y = -200;
          break;
        case 5:
          this.x = -100;
          this.y = -200;
          break;
        case 6:
          this.x = -200;
          this.y = -400;
          break;
        case 7:
          this.x = -200;
          this.y = -400;
          break;
        case 8:
          this.x = -100;
          this.y = -400;
          break;
        case 9:
          this.x = -100;
          this.y = -300;
          break;
      }
    },
    locate(point1, point2) {
      function rad(d) {
        return d * Math.PI / 180.0; //经纬度转换成三角函数中度分表形式。
      }
      let radLat1 = rad(point1.lat);
      let radLat2 = rad(point2.lat);
      let a = radLat1 - radLat2;
      let b = rad(point1.lng) - rad(point2.lng);
      let s =
        2 *
        Math.asin(
          Math.sqrt(
            Math.pow(Math.sin(a / 2), 2) +
              Math.cos(radLat1) *
                Math.cos(radLat2) *
                Math.pow(Math.sin(b / 2), 2)
          )
        );
      s = s * 6378.137;
      // EARTH_RADIUS;
      s = Math.round(s * 10000) / 10000;
      // console.log(s)
      return s;
    },
    narrowSpot(userlat, userlng) {
      const userPoint = {
        lat: userlat,
        lng: userlng
      };
      let distance1 = 0;
      let distance2 = 0;
      let nearSpot = 1;
      const self = this;
      for (let i = 0; i < this.spotList.length; i++) {
        let spotPoint = {
          lat: self.spotList[i].latitude,
          lng: self.spotList[i].longitude
        };
        if (i == 0) {
          distance1 = self.locate(userPoint, spotPoint);
        } else {
          distance2 = self.locate(userPoint, spotPoint);
        }
        if (distance1 < distance2) {
          distance1 = distance2;
          nearSpot = self.spotList[i].sortNo;
        }
      }
      this.nearSpot = nearSpot;
      // console.log(nearSpot)
    },
    getSpot() {
      const self = this;
      wx.request({
        url: config.base + "spot/list", //开发者服务器接口地址",
        data: {
          lineId: config.lineId
        }, //请求的参数",
        method: "GET",
        dataType: "json", //如果设为json，会尝试对返回的数据做一次 JSON.parse
        success: res => {
          // console.log(res)
          // self.GLOBAL.spot_list = res.data.data
          this.spotList = res.data.data;
          this.setStorage("spotList", res.data.data);
        },
        fail: () => {},
        complete: () => {}
      });
    },
    closeMessage() {
      this.showMessage = false;
    },
    touchMap() {
      console.log("touch map");
      this.active(-1);
    },
    // 调整地理坐标
    adjustLocation() {
      wx.getLocation({
        type: "wgs84", //默认为 wgs84 返回 gps 坐标，gcj02 返回可用于wx.openLocation的坐标,
        success: res => {
          console.info("getLocation success: ", res);
          const latitude = res.latitude;
          const longitude = res.longitude;
          const speed = res.speed;
          const accuracy = res.accuracy;
          // console.log("地理坐标", { latitude, longitude });
          this.lng = longitude;
          this.lat = latitude;
          this.isNear();
          return;
          // this.narrowSpot(latitude, longitude);

          // 测试
          this.abc++;
          let n = this.abc;
          console.log({ n }, this.abc);
          // this.narrowSpot(latitude + n * 0.001, longitude + n * 0.001);
          // n = 0;
          this.lng = this.mapStart.lng + n * 0.00001;
          this.lat = this.mapEnd.lat + n * 0.00001;
          // 10的位置
          this.lng = 114.496348;
          this.lat = 22.603917;
          // 1的位置
          this.lng = 114.496869;
          this.lat = 22.605782;
          // 4的位置
          this.lng = 114.498953;
          this.lat = 22.607684;
          console.log(this.lng, this.lat);
          this.isNear();
        },
        fail: () => {
          console.log("getLocation failed");
        }
      });
    },
    // 是否靠近了某个景点,如果是,则自动播放
    isNear() {
      if (this.spotList) {
        this.spotList.find(n => {
          let { longitude, latitude, sortNo } = n;
          const distance = 0.00001;
          if (
            this.lng &&
            this.lat &&
            Math.abs(this.lng - longitude) <= distance &&
            Math.abs(this.lat - latitude) <= distance &&
            this.activeSpot === -1 &&
            1
          ) {
            // 激活最近点
            this.active(sortNo - 1);
          }
        });
      }
    },
    // 激活景点
    active(index) {
      this.activeSpot = index;
      this.activeWindow = index;
      if (index === -1) {
        // 停止播放音乐
        this.playAudio();
      } else {
        //  自动播放音乐
        let spot_id = this.spotList.find(n => n.sortNo === index + 1).spot_id;
        console.log({ index, spot_id });
        wx.request({
          url: config.base + "spot/listdetail", //开发者服务器接口地址",
          data: {
            spot_id: spot_id,
            lineId: config.lineId
          }, //请求的参数",
          method: "GET",
          dataType: "json", //如果设为json，会尝试对返回的数据做一次 JSON.parse
          success: res => {
            let data = res.data.data;
            console.log(data);
            let audioUrl = config.prefix + res.data.data[0].audio_url;
            this.playAudio(audioUrl);
          }
        });
      }
    },
    playAudio(url) {
      if (url) {
        this.innerAudioContext.src = url;
        this.innerAudioContext.play();
      } else {
        this.innerAudioContext.stop();
      }
    }
  },
  created() {
    if (!this.innerAudioContext) {
      this.innerAudioContext = wx.createInnerAudioContext();
    }
    if (this.getStorage("spotList")) {
      this.spotList = this.getStorage("spotList");
    } else {
      this.getSpot();
    }
  },
  onShow() {
    console.log("onShow");
    this.adjustLocation();
    this._tForLocation = setInterval(() => {
      this.adjustLocation();
    }, 5000);
  },
  onHide() {
    console.log("on hide");
    if (this._tForLocation) {
      clearInterval(this._tForLocation);
    }
    this.innerAudioContext.stop();
    console.log("stop audio");
  },
  onUnload() {
    console.log("on unload");
    if (this._tForLocation) {
      clearInterval(this._tForLocation);
    }
    this.innerAudioContext.stop();
    console.log("stop audio");
  },
  mounted() {
    this.locate(this.mapStart, this.mapEnd);

    wx.getLocation({
      type: "wgs84", //默认为 wgs84 返回 gps 坐标，gcj02 返回可用于wx.openLocation的坐标,
      success: res => {
        console.info("getLocation success: ", res);
        const latitude = res.latitude;
        const longitude = res.longitude;
        const speed = res.speed;
        const accuracy = res.accuracy;
        let isOut =
          latitude < Math.min(this.mapStart.lat, this.mapEnd.lat) ||
          latitude > Math.max(this.mapStart.lat, this.mapStart.lat) ||
          longitude < Math.min(this.mapStart.lng, this.mapEnd.lng) ||
          longitude > Math.max(this.mapStart.lng, this.mapEnd.lng);
        if (isOut) {
          this.showMessage = true;
          setTimeout(() => {
            this.showMessage = false;
          }, 2000);
        }
      },
      fail: () => {
        console.log("getLocation failed");
      }
    });
  },
  onReady() {
    this.x = -100;
    this.y = -200;
  }
};
</script>

<style scoped lang="less">
.message {
  @w: 80vw;
  @h: 20vh;
  width: @w;
  height: @h;
  position: absolute;
  top: calc(50% - @h / 2);
  left: calc(50% - @w / 2);
  background-color: black;
  opacity: 0.6;
  color: white;
  padding: 5% 15% 0;
  box-sizing: border-box;
  &-close {
    position: absolute;
    @size: 50rpx;
    width: @size;
    height: @size;
    right: 20rpx;
    top: 20rpx;
    img {
      width: 100%;
      height: 100%;
    }
  }
}
.center {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
#mid {
  width: 10px;
  height: 10px;
  background: red;
  position: absolute;
  top: 1000rpx;
  left: 430rpx;
  z-index: 99999;
}
.container {
  width: 100vw;
  height: 100vh;
  position: relative;
  z-index: 2;
}
.map-tab {
  height: 150rpx;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
  display: flex;
  justify-content: space-around;
  align-items: center;
  background: #d5aa80;
  z-index: 99;
  display: none;
  &-item {
    @size: 110rpx;
    width: @size;
    height: @size+10rpx;
    img {
      width: 100%;
      height: 100%;
      display: block;
    }
  }
}
.index-bg {
  width: auto;
  height: auto;
  padding-bottom: 138rpx;
  background: #d1a77f;
  position: relative;
  .mapImg {
    // width: 1560rpx;
    // height: 2640rpx;
    width: 1180rpx;
    height: 2000rpx;
  }
}
.userIcon {
  width: 80rpx;
  height: 120rpx;
  position: absolute;
  z-index: 888;
}
.spot-window {
  position: absolute;
  z-index: 999;
  width: 248rpx;
  height: 248rpx;
  border: 6rpx solid #bc8d5d;
  // background: #00baea;
  background: url("https://gw.alicdn.com/tfs/TB1Mxiei4TpK1RjSZR0XXbEwXXa-248-248.png")
    no-repeat center/cover;
  border-radius: 40rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  &-text {
    font-size: 26rpx;
    color: #fff;
    line-height: 90rpx;
  }
  &-img {
    width: 220rpx;
    height: 144rpx;
    border-radius: 24rpx;
  }
}
.active-spot {
  width: 40rpx;
  height: 30rpx;
  margin: auto;
}
.spot-icon {
  width: 60rpx;
  height: 46rpx;
  line-height: 40rpx;
  background: url("https://gw.alicdn.com/tfs/TB1dOumi4TpK1RjSZFMXXbG_VXa-64-46.png")
    no-repeat center/cover;
  position: absolute;
  z-index: 888;
  text-align: center;
  color: #fff;
  font-size: 30rpx;
}
.icon-five {
  left: 810rpx;
  top: 880rpx;
}
.icon-99 {
  color: red;
}
.icon-1 {
  left: 500rpx;
  top: 770rpx;
}
.window-1 {
  left: 412rpx;
  top: 500rpx;
}
.icon-2 {
  left: 510rpx;
  top: 620rpx;
}
.window-2 {
  left: 412rpx;
  top: 350rpx;
}
.icon-3 {
  left: 525rpx;
  top: 500rpx;
}
.window-3 {
  left: 412rpx;
  top: 230rpx;
}
.icon-4 {
  left: 830rpx;
  top: 480rpx;
}
.window-4 {
  left: 730rpx;
  top: 210rpx;
}
.icon-5 {
  left: 806rpx;
  top: 870rpx;
}
.window-5 {
  left: 700rpx;
  top: 600rpx;
}
.icon-6 {
  left: 790rpx;
  top: 1100rpx;
}
.window-6 {
  left: 690rpx;
  top: 830rpx;
}
.icon-7 {
  left: 670rpx;
  top: 1430rpx;
}
.window-7 {
  left: 580rpx;
  top: 1160rpx;
}
.icon-8 {
  left: 870rpx;
  top: 1690rpx;
}
.window-8 {
  left: 770rpx;
  top: 1410rpx;
}
.icon-9 {
  left: 510rpx;
  top: 1480rpx;
}
.window-9 {
  left: 410rpx;
  top: 1200rpx;
}
.icon-10 {
  left: 400rpx;
  top: 1140rpx;
}
.window-10 {
  left: 300rpx;
  top: 870rpx;
}
.map-sub {
  position: fixed;
  top: 0;
  right: 25rpx;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  z-index: 66;
  &-line {
    width: 2px;
    height: 38rpx;
    background: #9e7044;
  }
  &-btn {
    @size: 80rpx;
    width: @size;
    height: @size;
    border-radius: 50%;
    border: 2rpx solid #9e7044;
    background: #f6ca47;
    .center();
    &-in {
      @insize: 0.76 * @size;
      width: @insize;
      height: @insize;
      border-radius: 50%;
      border: 2rpx solid #9e7044;
      background: #f7eec5;
      position: relative;
      .center();
      img {
        width: 65%;
        height: 65%;
      }
    }
  }

  // .btn-show {
  //   width: 30rpx;
  //   height: 30rpx;
  // }
  // .btn-audio {
  //   width: 34rpx;
  //   height: 28rpx;
  // }
}
</style>
