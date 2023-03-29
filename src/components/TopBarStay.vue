<template>
  <div class="TopBar" v-if="opview">
    <a
      href="https://www.baidu.com/s?wd=%E7%99%BE%E5%BA%A6%E7%83%AD%E6%90%9C&sa=ire_dl_gh_logo_texing&rsv_dl=igh_logo_pc"
      target="_blank"
      title="点击一下，了解更多"
      ><img src="../assets/百度常态.png" class="appear"
    /></a>
    <div id="search">
      <div id="bar">
        <input id="txt" ref="txt" type="text" v-model="v" @keyup.enter="know" />
        <span id="soutu">
          <img src="../assets/相机0.png" />
        </span>
        <span id="more">按图片搜索</span>
      </div>
      <input id="su" type="submit" value="百度一下" @click="know" />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      v: "",
      opview: false,
    };
  },
  methods: {
    know() {
      location.href = "https://www.baidu.com/s?wd=" + this.v;
    },
    handleScroll() {
      var scrollTop =
        document.documentElement.scrollTop;
      if (scrollTop > 184) {
        this.opview = true;
      } else {
        this.opview = false;
      }
    },
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);
  },
};
</script>

<style scoped>
.appear{
  animation: OP 1s;
}
@keyframes OP{
  from{
    opacity: 0;
  }
  to{
    opacity: 100%;
  }
}
.TopBar {
  display: flex;
  height: 70px;
  position: sticky;
  top: 0px;
  background-color: #fff;
  box-shadow: 0px 0px 10px -4px gray;
  z-index: 9999;
}
.TopBar a img {
  width: 128px;
  margin: -8px 0 0 15px;
}
.TopBar .search {
  height: 50px;
}
div #search {
  position: relative;
  margin: 14px 0 0 -40px;
}
div #bar:hover #txt {
  border: 2px solid rgb(172, 172, 172);
}
div #txt {
  position: absolute;
  height: 16px;
  width: 443px;
  left: 43px;
  padding: 10px 87px 10px 16px;
  border: 2px solid rgb(196, 199, 206);
  border-radius: 10px 0 0 10px;
  color: #222;
  box-sizing: content-box;
}
#search #soutu img {
  position: absolute;
  cursor: pointer;
  right: -576px;
  top: 5px;
  height: 30px;
}
#search #soutu img:hover {
  content: url("../assets/相机.png");
}
#search #soutu:hover + #more {
  display: block;
}
div #txt:hover {
  border: 2px solid rgb(172, 172, 172);
}
div #txt:focus {
  border: 2px solid rgb(80, 112, 242);
}
div #more {
  position: absolute;
  display: none;
  right: -588px;
  top: 48px;
  font-size: 14px;
  height: 32px;
  width: 94px;
  box-shadow: 0 2px 10px 0 rgba(0, 0, 0, 0.1);
  border-radius: 6px;
  line-height: 32px;
  color: #626675;
  text-align: center;
}
div #su {
  position: absolute;
  left: 589px;
  height: 40px;
  line-height: 41px;
  width: 108px;
  padding: 0;
  font-size: 17px;
  color: white;
  border-radius: 0 10px 10px 0;
  background-color: rgb(78, 110, 242);
  border: none;
  cursor: pointer;
}
div #su:hover {
  background-color: rgb(70, 98, 217);
}
</style>