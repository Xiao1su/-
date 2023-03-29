<template>
  <div class="end">
    <div class="content">
      <div class="text" ref="ab">
        <p><a class="isShow" href="#">关于百度</a></p>
        <p><a class="isShow" href="#">About Baidu</a></p>
        <p><a class="isShow" href="#">使用百度前必读</a></p>
        <p><a class="isShow" href="#">帮助中心</a></p>
        <p><a class="isShow" href="#">企业推广</a></p>
        <p><a class="isShow" href="#">京公网安备11000002000001号</a></p>
        <p><a class="isShow" href="#">京ICP证030173号</a></p>
        <p><a class="isShow" href="#">信息网络传播视听节目许可证 0110516</a></p>
        <p><a class="isShow" href="#">互联网宗教信息服务许可证编号：京（2022）0000043</a></p>
        <p><a class="isShow" href="#">药品医疗器械网络信息服务备案（京）网药械信息备字（2021）第00159号</a></p>
        <p><a class="isShow" href="#">医疗器械网络交易服务第三方平台备案凭证（京）网械平台备字（2020）第00002号</a></p>
        <p><a class="isShow" href="#">©2023 Baidu </a></p>
      </div>
        <span class="tu">@</span>
      <div class="icon" v-if="isShow">
        <span class="biao">^</span>
      </div>
      <div class="hid" ref="hidContent">
        <a v-for="(h,index) of hiddenArr" :key="index">{{ h.innerHTML }}</a>
      </div>
    </div>
  </div>
</template>

<script>
export default{
  data(){
    return{
      screenWidth: "",
      screenHeight: "",
      num:0,
      totalW:0,
      isShow:true,
      Arr:[],//所有底部元素
      hiddenArr:[]
    };
  },
  methods:{
    getHid(){
      this.hiddenArr = [];
    // 如果底部导航栏宽度不够，则显示拓展标
    // 获取底部显示出来的元素数
    this.totalW = 0;
    this.num = 0;
    for(var i = 0; i < 12; i++){
        this.totalW += this.Arr[i].offsetWidth;
        this.totalW += 14;
        this.num+=1;
        if(this.totalW >= this.$refs.ab.offsetWidth){
          this.num--;
          break;
        }
      }
      // 将显示不出来的元素展示在隐藏栏中
      for(var x = this.num; x < 12; x++){
        // console.log(this.Arr[x]);
        this.hiddenArr.push(this.Arr[x]);
      }
      if(!this.hiddenArr.length){
        this.isShow = false;
      }else{
        this.isShow = true;
      }
    }
  },
  mounted(){  
    this.Arr = document.getElementsByClassName('isShow');
    this.screenWidth = document.body.clientWidth; //监听页面缩放
    window.onresize = () => {
      return (() => {
        this.screenWidth = document.body.clientWidth;
      })();
    };
    this.getHid();
  },
  watch: {
    screenWidth() {
      //浏览器窗口变化时，重新设置隐藏栏内内容。
      this.getHid();
    }
}
}
</script>

<style>
.end{
    position:fixed;
    bottom: 0;
    height: 40px;
    width: 100%;
    background-color: #fff;
}
.end .content{
  display: flex;
    overflow: hidden;
    line-height: 40px;
    font-size: 12px;
    width: 90%;
    text-align: center;
    margin:2px auto 0 auto;
}
.end .content .text{
  flex: 9;
  overflow: hidden;
}
.end .content p{
    display: inline-block;
    margin-right: 14px;
}
.end .content p a{
  color: rgb(187, 187, 187);
}
.end .content p a:hover{
    color:rgb(34, 34, 34);
}
.end .content .icon{
  flex: 1;
  text-align: left;
  color:black;
  width: 10%;
}
.end .content .icon .biao{
  display: block;
  font-size: 18px;
  margin-left:10px;
  height: 40px;
  width: 40px;
  line-height: 44px;
  text-align: center;
  color:black;
  cursor: pointer;
}
.hid{
  display: none;
  flex-direction: column;
  position: absolute;
  text-align: left;
  width: 468px;
  bottom:34px;
  left: 60%;
  border-radius: 8px;
  box-shadow: 0px 0px 10px -4px gray;
  color: floralwhite;
  background-color: #fff;
  overflow: hidden;
}
.end .content .icon:hover+.hid,
.hid:hover{
  display: flex;
}
.hid a{
  margin:0 0 0 8px;
  line-height: 20px;
  height: 20px;
  color:rgb(98,102,117);
}
</style>