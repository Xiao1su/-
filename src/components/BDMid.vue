<template>
    <div class="middle">
        <a href="https://www.baidu.com/s?wd=%E7%99%BE%E5%BA%A6%E7%83%AD%E6%90%9C&sa=ire_dl_gh_logo_texing&rsv_dl=igh_logo_pc" target="_blank" title="点击一下，了解更多"><img src="../assets/百度常态.png"></a>
        <div class="search">
            <div class="bar">
            <input class="txt" ref="txt" type="text" v-model="v" @keyup.enter="know">
            <span class="soutu">
                <img src="../assets/相机0.png"/>
            </span>
            <span class="more">按图片搜索</span>
        </div>
            <input class="su" type="submit" value="百度一下" @click="know">
        </div>
    </div>
</template>

<script>

export default{
    name:'SouSuo',
    data(){
        return{
            v:'',
        }
    },
    methods:{
        know(){
            location.href='https://www.baidu.com/s?wd='+this.v;
        }
    },
    mounted(){
        // 挂载完毕后，搜索框自动获取焦点
        this.$refs.txt.focus();
    }
}
</script>

<style lang="css" scoped>
   .middle{
        display:block;
        position:relative;
        height: 270px;
        width:740px;
        overflow: hidden;
        margin:0 auto;
    }
    div img{
        position:relative;
        margin:-16px auto 11.5px auto;
        height: 129px;
    }
    div .search{
        position:relative;
    }
    div .bar:hover .txt{
        border: 2px solid rgb(172, 172, 172);
    }
    div .txt{
        position:absolute;
        height:16px;
        width:443px;
        left:43px;
        padding: 12px 87px 12px 16px;
        border: 2px solid rgb(196, 199, 206);
        border-radius: 10px 0 0 10px;
        color:#222;
        box-sizing: content-box;
    }
    .search .soutu img{
        position: absolute;
        cursor: pointer;
        right: 164px;
        top:23px;
        height: 30px;
    }
    .search .soutu img:hover{
        content:url('../assets/相机.png')
    }
    .search .soutu:hover+.more{
        display:block;
    }
    div .txt:hover{
        border: 2px solid rgb(172, 172, 172);
    }
    div .txt:focus{
        border: 2px solid rgb(80, 112, 242);
    }
    div .more{
        position: absolute;
        display: none;
        top:50px;
        right:154px;
        font-size: 14px;
        height: 32px;
        width: 94px;
        box-shadow: 0 2px 10px 0 rgba(0,0,0,.1);
        border-radius: 6px;
        line-height: 32px;
        color:#626675;
        text-align: center;
    }
    div .su{
        position:absolute;
        left: 589px;
        height:44px;
        line-height: 45px;
        width:108px;
        padding: 0;
        font-size: 17px;
        color:white;
        border-radius: 0 10px 10px 0;
        background-color: rgb(78, 110, 242);
        border: none;
        cursor:pointer;
    }
    div .su:hover{
        background-color: rgb(70,98,217);
    }
</style>