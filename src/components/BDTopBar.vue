<template>
  <div class="topBar">
    <ul class="left">
      <li><a href="https://news.baidu.com/" target="_blank">新闻</a></li>
      <li><a href="https://www.hao123.com/" target="_blank">hao123</a></li>
      <li><a href="https://map.baidu.com/" target="_blank">地图</a></li>
      <li><a href="https://tieba.baidu.com/" target="_blank">贴吧</a></li>
      <li><a href="https://haokan.baidu.com/" target="_blank">视频</a></li>
      <li><a href="https://image.baidu.com/" target="_blank">图片</a></li>
      <li><a href="https://pan.baidu.com/" target="_blank">网盘</a></li>
      <li class="more">
        <a href="https://www.baidu.com/more/" target="_blank">更多</a>
        <ul class="leftBox">
          <div class="box">
            <li>
              <a href="#"
                ><img src="../assets/BDTopbarPhoto/翻译.png" /><br />翻译</a
              >
            </li>
            <li>
              <a href="#"
                ><img src="../assets/BDTopbarPhoto/学术.png" /><br />学术</a
              >
            </li>
            <li>
              <a href="#"
                ><img src="../assets/BDTopbarPhoto/文库.png" /><br />文库</a
              >
            </li>
            <li>
              <a href="#"
                ><img src="../assets/BDTopbarPhoto/百科.png" /><br />百科</a
              >
            </li>
            <li>
              <a href="#"
                ><img src="../assets/BDTopbarPhoto/知道.png" /><br />知道</a
              >
            </li>
            <li>
              <a href="#"
                ><img src="../assets/BDTopbarPhoto/健康.png" /><br />健康</a
              >
            </li>
            <li>
              <a href="#"
                ><img src="../assets/BDTopbarPhoto/营销.png" /><br />营销推广</a
              >
            </li>
            <li>
              <a href="#"
                ><img src="../assets/BDTopbarPhoto/直播.png" /><br />直播</a
              >
            </li>
            <li>
              <a href="#"
                ><img src="../assets/BDTopbarPhoto/音乐.png" /><br />音乐</a
              >
            </li>
          </div>
          <span class="foot"><a href="#">查看全部百度产品></a></span>
        </ul>
      </li>
    </ul>
    <ul class="right">
      <li>
        <span
          ><a href="" @click="login">{{ this.log }}账户</a></span
        >
      </li>
      <li><a href="" @click="setLog">设置</a></li>
      <li class="info">
        <span
          ><a href=""
            >{{ this.DefaultCity }}
            <img src="../assets/BDTopbarPhoto/天气-晴.png" />
            {{ this.today.tem }} ℃
          </a>
          <span class="airLevel" ref="level">{{ today.air_level }}</span>
        </span>
      </li>
      <div class="rightBox">
        <div class="rightBox1" ref="rightBox1">
          <div class="rightTop">
            <span>{{ this.today.date }}</span>
            <span>湿度：{{ today.humidity }}</span>
            <div @click="changeCity" class="dingWei">
              <span>切换地区</span>
            </div>
          </div>
          <div class="rightContent">
            <ul>
              <li v-for="(w, index) of WeatherInfo.slice(0, 5)" :key="index">
                <div class="contentTop">{{ w.week }}</div>
                <img src="../assets/BDTopbarPhoto/百科.png" />
                <div>{{ w.tem2 }}~{{ w.tem1 }}</div>
                <div>{{ w.wea }}</div>
                <div>{{ w.win_speed }}</div>
              </li>
            </ul>
          </div>
        </div>
        <div ref="rightBox2">
          <div class="changeContent" ref="toTop">
            <div>请选择您所在的地区：</div>
            <select name="cars" ref="city">
              <option value="太原">太原</option>
              <option value="北京">北京</option>
              <option value="上海">上海</option>
              <option value="杭州">杭州</option>
              <option value="乌鲁木齐">乌鲁木齐</option>
            </select>
            <div>
              <button @click="saveCity(false)">取消</button>
              <button @click="saveCity(true)">保存</button>
            </div>
          </div>
        </div>
      </div>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "pLogin",
  data() {
    return {
      log: localStorage.getItem("log", "登录") ? "退出" : "登录",
      Weathers: [],
      WeatherInfo: [],
      today: "",
      DefaultCity: "太原",
    };
  },
  methods: {
    // 保存本地登录信息
    setLog() {
      if (localStorage.getItem("msg", "123")) {
        localStorage.removeItem("msg", "123");
        localStorage.removeItem("log", "666");
        alert("已删除本地信息");
        // location.reload();
      } else {
        localStorage.setItem("msg", "123");
        alert("已保存本地信息，点击登录账户即可登录");
        // location.reload();
      }
    },
    // 点击登录后触发登录事件
    login() {
      if (
        localStorage.getItem("msg", "123") &&
        localStorage.getItem("log", "登录")
      ) {
        localStorage.removeItem("log", "登录");
        this.log = "登录";
        alert("退出成功");
      } else if (
        localStorage.getItem("msg", "123") &&
        !localStorage.getItem("log", "登录")
      ) {
        localStorage.setItem("log", "登录");
        this.log = "退出";
        alert("登录成功");
      } else {
        alert("登录失败");
      }
    },
    getWeather() {
      axios
        .get(
          "https://v0.yiketianqi.com/api?unescape=1&version=v91&appid=43656176&appsecret=I42og6Lm&ext=&cityid=&city=" +
            this.DefaultCity
        )
        .then(
          (response) => {
            this.Weathers = response.data;
            this.WeatherInfo = response.data.data;
            this.today = this.WeatherInfo[0];
            if (this.today.air_level == "优") {
              this.$refs.level.style.backgroundColor = "rgb(78, 237, 78)";
            } else if (this.today.air_level == "轻度污染") {
              this.$refs.level.style.backgroundColor = "rgb(247,103,7)";
            } else if (this.today.air_level == "重度污染") {
              this.$refs.level.style.backgroundColor = "red";
            }else{
              this.$refs.level.style.backgroundColor = "rgb(130, 201, 30)";
            }
          },
          (error) => {
            console.log("天气预报请求失败", error.data);
          }
        );
    },
    changeCity() {
      this.$refs.toTop.style.display = "block";
    },
    saveCity(e) {
      if (e) {
        this.DefaultCity = this.$refs.city.value;
        this.getWeather();
      }
      this.$refs.toTop.style.display = "none";
    },
  },
  mounted() {
    this.getWeather();
  },
};
</script>

<style lang="css" scoped>
.topBar {
  position: relative;
  height: 60px;
  width: 100%;
}
.left {
  width: 500px;
}
.left li {
  float: left;
  margin: 23px 0 0 24px;
  font-size: 13px;
  color: grey;
}
.left .more {
  margin-top: 18px;
  padding: 5px 0 19px;
}
.left li a {
  text-align: center;
  margin: 0;
  color: rgb(40, 40, 40);
}
.left li a:hover {
  color: rgb(10, 30, 220);
}
.left .more:hover .leftBox,
.left .more .leftBox:hover {
  display: block;
}
.leftBox {
  box-shadow: 0px 0px 10px -4px gray;
  border-radius: 16px;
  width: 228px;
  height: 298px;
  background-color: white;
  float: left;
  margin: 10px 0 0 -190px;
  position: absolute;
  display: none;
  z-index: 999;
}
.leftBox .foot {
  float: left;
  width: 90%;
  height: 28px;
  line-height: 28px;
  margin: 5px 5% 0 5%;
  border-radius: 5px;
}
.leftBox .foot:hover a {
  color: rgb(10, 30, 220);
}
.leftBox .foot:hover {
  background-color: rgb(229, 243, 255);
}
.box {
  display: flex;
  margin-top: 8px;
  flex-wrap: wrap;
  justify-content: space-evenly;
}
.leftBox li {
  display: flex;
  height: 82px;
  width: 60px;
  justify-content: center;
  align-items: center;
  font-size: 13px;
  margin: 0;
  border-radius: 5px;
  padding: 0;
}
.leftBox li a {
  height: 82px;
  width: 60px;
  text-align: center;
  justify-content: center;
}
.leftBox li a img {
  height: 42px;
  border-radius: 10%;
  margin: 8px 0 4px 0;
}
.leftBox li:hover {
  background-color: rgb(241, 243, 253);
}
.leftBox li:hover a {
  color: rgb(10, 30, 220);
}
.leftBox .foot a {
  color: rgb(136, 136, 136);
}
.leftBox img {
  height: 32px;
}
.right {
  height: 60px;
  margin: 0;
  right: 20px;
  position: absolute;
}
.right li {
  float: right;
  margin: 23px 24px 0 0;
  font-size: 13px;
  color: grey;
}
.right li img {
  height: 15px;
}
.right .airLevel {
  height: 18px;
  border-radius: 3px;
  padding: 0 2px;
  font-size: 12px;
  color: #fff;
}
.right .info {
  height: 25px;
  margin-top: 21px;
  padding-bottom: 10px;
}
.right .info:hover + .rightBox,
.rightBox:hover {
  display: block;
}
.rightBox {
  display: none;
  box-shadow: 0px 0px 10px -4px gray;
  border-radius: 16px;
  width: 464px;
  height: 170px;
  background-color: #fff;
  margin: 48px 0 0 -254px;
  padding: 12px 16px 16px;
  position: absolute;
  z-index: 9999;
}
.rightBox .rightTop {
  height: 24px;
  width: 464px;
  background-color: #fff;
}
.rightBox .rightTop span {
  margin-left: 16px;
  float: left;
}
.rightBox .rightTop .dingWei {
  float: right;
  cursor: pointer;
}
.rightBox .rightContent {
  height: 146px;
  width: 464px;
}
.rightBox .rightContent ul li {
  float: left;
  height: 123px;
  width: 84px;
  margin: 13px 11px 0 0;
  padding: 5px 0;
  border-radius: 6px;
  color: rgb(51, 51, 51);
}
.rightBox .rightContent ul li:hover {
  background-color: rgb(245, 245, 246);
}
.rightBox .rightContent ul li:last-child {
  margin-right: 0px;
}
.rightBox .rightContent .contentTop {
  height: 23px;
  width: 84px;
}
.rightBox .rightContent ul li img {
  height: 32px;
  margin: 3px 26px;
}
.rightBox .rightContent ul li div {
  height: 22px;
  width: 84px;
}
.topBar {
  line-height: 15px;
}
.rightBox .changeContent {
  display: none;
  position: absolute;
  left: 0;
  top: 0;
  padding-top: 40px;
  height: 146px;
  width: 100%;
  border-radius: 16px;
  flex-direction: column;
  background-color: #fff;
}
.rightBox .changeContent select {
  width: 150px;
  margin: 20px auto;
}
.rightBox .changeContent button:first-child {
  margin-right: 30px;
}
</style>