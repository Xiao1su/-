<template>
  <div class="low">
    <button class="btn" @click="Hot">获取最新热点</button>
      <div class="left">
        <span @click="choose($event)" class="select" ref="wo">我的关注</span>
        <span @click="choose($event)" class="select active" ref="tui"
          >推荐</span
        >
      </div>
      <div class="right" v-show="focusShow">
        <span class="aa">
          <a class="bdhot" href="https://top.baidu.com"
            >百度热搜
            <span>></span>
          </a>
          <a class="change" @click="changeHot">
            <img
              @animationend="rotateEnd"
              ref="shua"
              :class="{ rotate180: showAnimate }"
              src="../assets/刷新0.png"
            />
            <span>换一换</span>
          </a>
        </span>
      </div>
      <span class="right" v-show="!focusShow">
        <a class="change">
          <span class="fuzi">
            <span class="fu">+</span>
            <span class="zi">自定义</span>
          </span>
        </a></span
      >
    <div class="content" v-show="focusShow">
      <div class="news">
        <ul>
          <li v-for="(n, index) of News" :key="index">
            <div class="oneNew">
              <div class="photo">
                <a :href="n.url" :title="n.title"
                  ><img :src="n.picUrl" alt="加载中" /><br
                /></a>
              </div>
              <div class="txt">
                <a class="title" :href="n.url" :title="n.title">{{
                  n.title
                }}</a>
                <div class="info">
                  <a
                    class="source"
                    :href="
                      'https://www.baidu.com/s?wd=' +
                      n.source +
                      '&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1'
                    "
                    >{{ n.source }}</a
                  >
                  <p class="ctime">{{ n.ctime }}</p>
                </div>
              </div>
            </div>
          </li>
        </ul>
      </div>
      <div class="hot">
        <ul>
          <li
            class="lie"
            v-for="(h, index) of HotSearch.slice(0,30)"
            :key="index" 
          >
            <a
              :href="
                'https://www.baidu.com/s?wd=' +
                h.keyword +
                '&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1'
              "
            >
              <span class="num">{{ HotSearch.indexOf(h) }}</span>
              <!-- 当前面数字为两位数时，与后方文字相距过近，所以增加一个6px宽的间隔 -->
              <span v-if="HotSearch.indexOf(h) > 9" style="margin-left:6px;"></span>
              <span v-if="HotSearch.indexOf(h)==0" style="color:rgb(246,48,81);font-size: 18px;position: absolute;left: 1px;">^</span>
              <span class="hotTitle" ref="title">{{ h.keyword }}</span>
              <!-- 文字后方的热度显示 -->
              <span class="trend" v-if="h.trend == '热' && HotSearch.indexOf(h) != 0">热</span>
            </a>
          </li>
        </ul>
      </div>
    </div>
    <div class="myfocus" v-show="!focusShow && loggin">
      <div class="wrapper">
        <span>我的导航</span>
        <span>您还没有添加任何网址</span>
      </div>
    </div>
    <div class="myfocus" v-show="!focusShow && !loggin">
      <div class="wrapper">
        <span>您未登录账户，请点击右上角设置进行信息存储，再点击登录账户</span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "TodayNews",
  data() {
    return {
      focusShow: true,
      loggin: false,
      // hotStart: 0, //一共有31个新闻，使用这两个变量进行遍历
      // hotEnd: 30,
      hlie:0,//height lie  高度
      ll:360,
      showAnimate: false,
      News: [
        {
          id: "d77fb0b7a1d1d187b824e20cb98ba720",
          picUrl:
            "https://img0.utuku.imgcdc.com/300x200/news/20230320/bf6caff5-e0ed-45d1-90fa-53150dd531ea.png",
          ctime: "2023-03-20 00:00",
          title: "盲目攀比！凉山“80后”女书记贪图享乐坠入深渊",
          description: "",
          source: "中华社会",
          url: "https://news.china.com/social/1007/20230320/44704723.html",
        },
        {
          id: "ffa3c770f0f3e79d51bca077c4c253de",
          picUrl:
            "https://img1.utuku.imgcdc.com/300x200/news/20230319/e741d3ec-9c26-4d97-8a61-caef38855b50.jpg",
          ctime: "2023-03-19 00:00",
          title: "频繁收到营销短信，只能手动删除吗",
          description: "",
          source: "中华社会",
          url: "https://news.china.com/social/1007/20230319/44694370.html",
        },
        {
          id: "cbc5c8ea1c1909bd62d256e9c8e271a0",
          picUrl:
            "https://img2.utuku.imgcdc.com/300x200/news/20230320/fdc44ad7-6fdb-4336-9eb8-03b0f294e337.jpg",
          ctime: "2023-03-20 00:00",
          title: "警惕！非法电子烟酷似儿童玩具分销各地",
          description: "",
          source: "中华社会",
          url: "https://news.china.com/social/1007/20230320/44701163.html",
        },
        {
          id: "06c7002ef435e85623e5a9f50d4e0b38",
          picUrl:
            "https://img2.utuku.imgcdc.com/300x200/news/20230316/0e90ac0e-ebd6-4fe8-893a-45bcfbcf971a.png",
          ctime: "2023-03-16 00:00",
          title: "3·15曝光不可注射的妆字号美容针或致毁容",
          description: "",
          source: "中华社会",
          url: "https://news.china.com/social/1007/20230316/44679041.html",
        },
        {
          id: "4331a8083ed9ba4bef60b3b4dbaad573",
          picUrl:
            "https://img1.utuku.imgcdc.com/300x200/news/20230316/b7085639-0dc7-4df7-8967-952e1a5b1983.png",
          ctime: "2023-03-16 00:00",
          title: "你在直播间买的酒为啥那么便宜？",
          description: "",
          source: "中华社会",
          url: "https://news.china.com/social/1007/20230316/44682750.html",
        },
        {
          id: "65ad049c6456cba6d4f3b8cfccd80f71",
          picUrl:
            "https://img2.utuku.imgcdc.com/300x200/news/20230312/175b1571-139c-48af-a6da-a2e939771fcc.octet-stream",
          ctime: "2023-03-12 00:00",
          title: "医疗反腐触及基层，多地县医院“一把手”被查",
          description: "",
          source: "中华社会",
          url: "https://news.china.com/social/1007/20230312/44647060.html",
        },
        {
          id: "869c1168437d0626f427b97faabf2f07",
          picUrl:
            "https://img0.utuku.imgcdc.com/300x200/news/20230302/bc414488-51c2-457c-8aa9-48f5370fb9f5.jpg",
          ctime: "2023-03-02 00:00",
          title: "它时隔6年，再现身！网友：太可爱了吧……",
          description: "",
          source: "中华社会",
          url: "https://news.china.com/social/1007/20230302/44576038.html",
        },
        {
          id: "3fc1ef91c918015e382984d9d84659a8",
          picUrl:
            "https://img2.utuku.imgcdc.com/300x200/news/20230303/cfb32c89-3704-4505-8658-7b7efe40f869.jpg",
          ctime: "2023-03-03 00:00",
          title: "已有多人中招！公安部发布重要提醒",
          description: "",
          source: "中华社会",
          url: "https://news.china.com/social/1007/20230303/44585647.html",
        },
        {
          id: "614c36f7f54ae207cc804d567ecf8374",
          picUrl:
            "https://img3.utuku.imgcdc.com/300x200/news/20230320/146575fd-f9b3-4585-b176-ff3af6c0c7e6.jpg",
          ctime: "2023-03-20 00:00",
          title: "书画家孙其峰在天津逝世 享年104岁",
          description: "",
          source: "中华社会",
          url: "https://news.china.com/social/1007/20230320/44704620.html",
        },
        {
          id: "8e11617140c598784bfa49f62854afdc",
          picUrl:
            "https://img3.utuku.imgcdc.com/300x200/news/20230301/d736f9b5-3702-4f39-a4a1-bd9d67d0b9ae.jpg",
          ctime: "2023-03-01 00:00",
          title: "中学招投标现单价3600元插线板 当地已成立调查组",
          description: "",
          source: "中华社会",
          url: "https://news.china.com/social/1007/20230301/44566476.html",
        },
      ],
      HotSearch: [
        {
          keyword: "习近平“典”论中俄关系",
          brief:
            "应俄总统普京邀请，习近平主席对俄罗斯进行国事访问。关于中俄关系，早在10年前的演讲中，习主席便指出“中俄关系一定能够继续乘风破浪、扬帆远航”。 查看更多&gt;",
          index: "4975199",
          trend: "热",
        },
        {
          keyword: "中俄联合声明:用和谈解决乌克兰危机",
          brief:
            "3月21日，中俄元首共同签署并发表《中华人民共和国和俄罗斯联邦关于深化新时代全面战略协作伙伴关系的联合声明》，强调通过和谈解决乌克兰危机。 查看更多&gt;",
          index: "4942038",
          trend: "热",
        },
        {
          keyword: "习近平：两国关系远远超出双边范畴",
          brief:
            "当地时间21日，国家主席习近平同俄罗斯总统普京会谈后共同会见记者。习近平强调，中俄两国关系远远超出双边范畴，对世界格局和人类前途命运至关重要。 查看更多&gt;",
          index: "4894024",
          trend: "热",
        },
        {
          keyword: "春季旅游消费强势复苏",
          brief:
            "春风起，春日暖，春花次第开，中国多地迎来踏春好时节。赏花成为春季旅游的热门主题，以赏花为主线的旅游产品也正在成为乡村振兴的新力量。 查看更多&gt;",
          index: "4732455",
          trend: "热",
        },
        {
          keyword: "普京：我们有点羡慕中国",
          brief:
            "普京在3月21日表示：“我们都有点羡慕中国，中国取得了巨大的发展成就，引起了全世界的关注”。 查看更多&gt;",
          index: "4612630",
          trend: "",
        },
        {
          keyword: "北京：停止露天集会减少外出",
          brief: "查看更多&gt;",
          index: "4523770",
          trend: "",
        },
        {
          keyword: "北方再迎大范围沙尘",
          brief:
            "3月以来的第三轮沙尘天气目前正在影响我国北方地区。3月22日，北京、天津、河北等地将迎来沙尘影响的核心时段。 查看更多&gt;",
          index: "4487740",
          trend: "热",
        },
        {
          keyword: "买房是否要加快出手",
          brief: "查看更多&gt;",
          index: "4396364",
          trend: "",
        },
        {
          keyword: "闰二月可以上坟吗？",
          brief: "查看更多&gt;",
          index: "4208715",
          trend: "",
        },
        {
          keyword: "网友晒壮观的俄罗斯护送队",
          brief:
            "当地时间3月21日，网友拍下壮观的俄罗斯护送队，当五星红旗出现时，民族自豪感从心中升起。 查看更多&gt;",
          index: "4186248",
          trend: "热",
        },
        {
          keyword: "本世纪仅有4次 闰二月为何少见？",
          brief:
            "3月22日是癸卯兔年闰二月初一。天文科普专家表示，作为一种历法现象，闰二月比较少见。就21世纪这100年来说，闰二月一共有4次。 查看更多&gt;",
          index: "4076489",
          trend: "",
        },
        {
          keyword: "实拍北京沙尘暴：能见度小于1公里",
          brief:
            "北京市气象台3月22日05时40分升级发布沙尘暴黄色预警信号：目前本市已出现明显沙尘天气，预计当前至22日中午，大部分地区最低能见度小于1000m。 查看更多&gt;",
          index: "3971064",
          trend: "",
        },
        {
          keyword: "开门杀致人死亡女子被判9个月",
          brief:
            "近日，如东法院开庭审理了一起因开车门致人伤亡的交通肇事案件。被告人吴某犯交通肇事罪，被判处有期徒刑九个月。 查看更多&gt;",
          index: "3813652",
          trend: "",
        },
        {
          keyword: "特朗普未“如期”被捕",
          brief: "查看更多&gt;",
          index: "3704914",
          trend: "",
        },
        {
          keyword: "邮储银行回应储户243万存款被挪用",
          brief:
            "近日，储户243万元存款被挪用要求赔偿被拒一事引发关注。对此，中国邮政集团有限公司南京市分公司21日发布声明称，案件正在重审中，将严格执行判决。 查看更多&gt;",
          index: "3668933",
          trend: "",
        },
        {
          keyword: "闰二月“双龙抬头”：吸龙气吃3样",
          brief:
            "2023年兔年是一个难得的“双春闰二月”年，而闰月的二月二老辈人则称之为“双龙抬头”。 按照老传统，要记得“吸龙气，吃3样龙食”的龙抬头习俗。 查看更多&gt;",
          index: "3525770",
          trend: "",
        },
        {
          keyword: "公交司机急刹车致乘客死亡获刑",
          brief: "查看更多&gt;",
          index: "3499724",
          trend: "",
        },
        {
          keyword: "孙彩瑛因衬衫图案发文道歉",
          brief: "查看更多&gt;",
          index: "3343899",
          trend: "",
        },
        {
          keyword: "挂上银牌后全红婵哭了",
          brief:
            "21日，全国跳水冠军赛暨奥运会、世锦赛、亚运会、大运会选拔赛迎来女子单人十米台决赛，全红婵获得亚军。赛后，全红婵哭了，并表示没有发挥到最好。 查看更多&gt;",
          index: "3213734",
          trend: "",
        },
        {
          keyword: "双胞胎萌娃打架道歉成相声现场",
          brief:
            "近日，辽宁一对双胞胎兄弟因为互相嫌弃画画不好打了起来。随后二人互相道歉的场景犹如相声现场。 查看更多&gt;",
          index: "3114343",
          trend: "",
        },
        {
          keyword: "蔡英文将过境美国见美政客?中方回应",
          brief:
            "有消息指出，蔡英文将于3月29日启程访问危地马拉，并在纽约和洛杉矶过境，与麦卡锡会面。21日，汪文斌回应：坚决反对美台官方往来，已提出交涉。 查看更多&gt;",
          index: "3037907",
          trend: "",
        },
        {
          keyword: "男子将200万房子80元“卖”了",
          brief:
            "儿子欠老父亲一百多万拆迁安置利益折价款，为逃避债务把名下房产以80元的价格卖给亲戚。法官识破并最终将这套房产上架拍卖，维护了老人的合法权益。 查看更多&gt;",
          index: "2900826",
          trend: "",
        },
        {
          keyword: "山东人上床睡觉时间全国最早",
          brief: "查看更多&gt;",
          index: "2816363",
          trend: "",
        },
        {
          keyword: "货车撞上“风水树” 村民索赔8.8万",
          brief:
            "近日，李女士聘请的司机在驾驶货车时，将路旁一棵树的树枝撞断。当地村民称该树为“风水树”，需赔偿8.8万元。目前，双方协商不下，已走法律途径。 查看更多&gt;",
          index: "2741379",
          trend: "",
        },
        {
          keyword: "杭州拟进一步放宽落户政策",
          brief: "查看更多&gt;",
          index: "2686292",
          trend: "",
        },
        {
          keyword: "李亚鹏：我不是王子所以没有王妃",
          brief: "查看更多&gt;",
          index: "2566377",
          trend: "",
        },
        {
          keyword: "应勇已任中央政法委委员",
          brief:
            "中国长安网“领导机构”栏目信息显示，应勇已出任中央政法委委员。此前，据新华社3月11日消息，应勇当选为最高人民检察院检察长。 查看更多&gt;",
          index: "2407464",
          trend: "",
        },
        {
          keyword: "岸田抵达基辅会见泽连斯基",
          brief:
            "综合日媒报道，当地时间21日，日本首相岸田文雄改变从访问地印度回国的计划，经由波兰换乘飞机和火车前往乌克兰首都基辅，会见乌总统泽连斯基。 查看更多&gt;",
          index: "2308736",
          trend: "",
        },
        {
          keyword: "中科大回应造黄谣男生能否录取",
          brief:
            "3月21日，南华大学学生何某某因在网络贩卖淫秽信息被开除学籍。中科大招生办表示，已发函询问南华大学是否保留何某某推免资格。 查看更多&gt;",
          index: "2241552",
          trend: "热",
        },
        {
          keyword: "睡眠质量不好会让人变笨变丑吗？",
          brief:
            "3月21日是世界睡眠日，睡眠质量不好会让人变笨变丑，是真的吗？睡不好的人容易变胖？快来听听专家怎么说。 查看更多&gt;",
          index: "2147205",
          trend: "",
        },
        {
          keyword: "8小时睡眠论可能是错的",
          brief:
            "大家都认为每天睡够8小时就是健康的睡眠，可是为什么很多人睡够8小时后身体反而更疲劳。实际上睡得好与睡眠时间长短并无必然关系。 查看更多&gt;",
          index: "2063648",
          trend: "",
        },
      ],
    };
  },
  methods: {
    Hot() {
      axios
        .get(
          "https://apis.tianapi.com/nethot/index?key=b80f39ea59877c65bcc9f024382648a3"
        )
        .then(
          (response) => {
            // console.log("右侧请求成功", response.data);
            this.HotSearch = response.data.result.list;
          },
          (error) => {
            console.log("右侧请求失败", error.message);
          },
          axios
            .get(
              "https://apis.tianapi.com/guonei/index?key=b80f39ea59877c65bcc9f024382648a3&n=10"
            )
            .then(
              (response) => {
                // console.log("左侧请求成功", response.data);
                this.News = response.data.result.newslist;
              },
              (error) => {
                console.log("左侧请求失败", error.message);
              }
            )
        );
    },
    changeHot() {
      // if (this.hotEnd != 30) {
      //   this.hotStart += 10;
      //   this.hotEnd += 10;
      // } else {
      //   this.hotStart = 0;
      //   this.hotEnd = 10;
      // }  之前实现的每次展示10个新闻，点击换一换按钮，展示后面10个
      // 下面为同时展示30个，但是有20个被hidden
      const hots = document.getElementsByClassName('lie');
      this.hlie == 720 ? this.hlie = 0 : this.hlie += 360;
      hots[0].style.margin = -this.hlie+'px 0 0 0';
      this.showAnimate = true;
    },
    rotateEnd() {
      this.showAnimate = false;
    },
    choose(e) {
      this.$refs.wo.classList.value = "select";
      this.$refs.tui.classList.value = "select";
      e.srcElement.classList.add("active");
      if (this.$refs.tui.classList.value == "select") {
        this.focusShow = false;
      } else {
        this.focusShow = true;
      }
    },
  },
  mounted() {
    // 判断本地是否有登录信息
    if (localStorage.getItem("msg", "123") && localStorage.getItem("log", "登录")) {
      this.loggin = true;
    }
    // 为前四个热点添加独有样式
    const hots = document.getElementsByClassName('num');
    hots[0].innerHTML = 'T';
    hots[0].style.color = 'rgb(246,48,81)';
    hots[1].style.color = 'rgb(254,45,70)';
    hots[2].style.color = 'rgb(255,102,0)';
    hots[3].style.color = 'rgb(250,169,29)';
    const hotWs = document.getElementsByClassName('hotTitle');
    // title的最大长度为272-22-4 = 246
    // 热度图标的显示长度为 8+16 = 24
    // 当title为222 时，热度恰好显示
    // 当title为246-8 = 238时，图标恰好不显示
    for(var i = 0; i < 30; i++){
      if(hotWs[i].offsetWidth >= 222){
        hotWs[i].style.width = '238px';
      }
    }
  }
}
</script>

<style lang="css" scoped>
.low {
  display: flex;
  position: relative;
  flex-direction: column;
  font-size: 14px;
  width: 944px;
  margin: 0 auto;
}
.low .btn {
  width: 100px;
  top: -40px;
  height: 30px;
  position: absolute;
}
.low .left {
  height: 30px;
  line-height: 30px;
  width: 672px;
  position: absolute;
  left: 0px;
  user-select: none;
}
.low .left .active {
  color: #343434;
  display: block;
  border-bottom: 2px solid rgb(78, 113, 242);
}
.low .left span {
  float: left;
  display: block;
  color: rgb(145, 149, 163);
  height: 26px;
  line-height: 26px;
  margin: 2px 24px 0 0;
  cursor: pointer;
}
.low .right {
  position: sticky;
  top: 106px;
  height: 30px;
  line-height: 30px;
  width: 272px;
  margin-left: 672px;
  text-align: left;
  user-select: none;
}
.bdhot {
  color: black;
  font-size: 14.6px;
  font-weight: bold;
  margin-right:143px;
}
.bdhot:hover {
  color: rgb(49, 94, 251);
}
.rotate180 {
  animation: rotate180 0.2s ease-out 0s;
}
@keyframes rotate180 {
  100% {
    transform: rotate(180deg);
  }
}
.change {
  color: rgb(98, 102, 117);
  cursor: pointer;
}
.change:hover {
  color: rgb(49, 94, 251);
}
.change img {
  position: relative;
  height: 16px;
  width: 16px;
  top: 3px;
  right: 5px;
}
.low .right .fu {
  height: 28px;
  font-size: 26px;
  line-height: 28px;
  margin-left: 214px;
}
.low .right .zi {
  position: relative;
  top: -3px;
}
.change:hover img {
  content: url("../assets/刷新.png");
  position: relative;
  height: 16px;
  top: 3px;
  right: 5px;
}
.content {
  display: flex;
}
.myfocus {
  width: 944px;
  height: 408px;
}
.myfocus .wrapper {
  display: flex;
  flex-direction: column;
  position: absolute;
  background-color: #fff;
  text-align: left;
  height: 280px;
  width: 976px;
  margin: 8px 0 0 -17px;
  border: 1px solid rgba(0, 0, 0, 0.05);
  border-radius: 12px;
  box-shadow: 0 2px 8px 0 rgba(0, 0, 0, 0.1);
}
.myfocus .wrapper span {
  position: relative;
  left: 16px;
  top: 13px;
  margin-bottom: 20px;
  color: #343434;
}
.news {
  flex: 2;
  margin: -7px 0 0 0;
  color: rgb(87, 87, 170);
}
.oneNew {
  display: flex;
  padding: 18px 0;
}
.oneNew .title {
  font-size: 20px;
  width: 360px;
  text-align: left;
  flex-wrap: wrap;
  color: #222;
  padding-left: 12px;
}
.oneNew .title:hover {
  color: rgb(49, 94, 251);
}
.oneNew .photo {
  position: relative;
  height: 122px;
  width: 184px;
  overflow: hidden;
  border: 1px solid rgba(0, 0, 0, 0.03);
  border-radius: 12px;
}
.oneNew .photo img {
  text-align: center;
  height: 100%;
  width: 100%;
}
.oneNew .txt {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 122px;
  margin-top: 3px;
}
.oneNew .info {
  height: 23px;
  width: 372px;
  display: flex;
  position: relative;
  font-size: 13px;
  padding-left: 12px;
}
.oneNew .info .source {
  line-height: 16px;
  margin-right: 12px;
  color: #626675;
}
.oneNew .ctime {
  line-height: 16px;
  cursor: default;
  color: #9195a3;
}
.hot {
  display: flex;
  flex-direction: row;
  margin-top: -1px;
  position: sticky;
  top: 135px;
  height: 360px;
  line-height: 36px;
  overflow: hidden;
}
.hot li a {
  display: flex;
  height: 36px;
  width: 272px;
  align-items: center;
  overflow: hidden;
}
.hot li a:hover .hotTitle {
  color: rgb(49, 94, 251);
  text-decoration: underline;
}
.hot li .num {
  height: 36px;
  width: 22px;
  min-width: 22px;
  color: rgb(145, 149, 163);
  font-size: 18px;
  text-align: left;
  margin-right: 4px;
}
.hot li .hotTitle {
  flex-shrink:0;
  font-size: 16px;
  height: 18px;
  max-width: 240px;
  line-height: 16px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  text-align: left;
  color: #222;
}
.hot li .trend {
  position: relative;
  background-color: rgb(255, 102, 0);
  color: #fff;
  height: 16px;
  width: 16px;
  line-height: 16px;
  font-size: 12px;
  border-radius: 4px;
  margin-left: 8px;
  top: -1px;
}
</style>