<template>
  <div id="app">
    <div v-show="!isMobile">
      <div class="headerBg"></div>
      <div class="content">
        <div class="contentImgFather">
          <img class="contentImg" src="../src/assets/img-1.jpg" alt />
        </div>
        <div class="contentImgFather">
          <img class="contentImg" src="../src/assets/img-1.jpg" alt />
        </div>
        <div class="contentImgFather">
          <img class="contentImg" src="../src/assets/img-1.jpg" alt />
        </div>
        <div class="contentImgFather">
          <img class="contentImg" src="../src/assets/img-1.jpg" alt />
        </div>
        <div class="contentImgFather">
          <img class="contentImg" src="../src/assets/img-1.jpg" alt />
        </div>

        <div class="smallImgFather">
          <img class="smallImg" src="../src/assets/img-1.jpg" alt />
          <img class="smallImg" src="../src/assets/img-1.jpg" alt />

          <img class="smallImg" src="../src/assets/img-1.jpg" alt />
          <img class="smallImg" src="../src/assets/img-1.jpg" alt />
        </div>
      </div>
    </div>

    <div class="popBg">
      <img class="img" src="@/assets/scape.png" alt />
      <div class="name">请竖屏手机</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      isMobile: false,
      imgArr:[],
      imgSmallArr:[]

    };
  },
  mounted() {

    var browser = {
      versions: (function () {
        var u = navigator.userAgent,
          app = navigator.appVersion;
        return {
          //移动终端浏览器版本信息
          trident: u.indexOf("Trident") > -1, //IE内核
          presto: u.indexOf("Presto") > -1, //opera内核
          webKit: u.indexOf("AppleWebKit") > -1, //苹果、谷歌内核
          gecko: u.indexOf("Gecko") > -1 && u.indexOf("KHTML") == -1, //火狐内核
          mobile: !!u.match(/AppleWebKit.*Mobile.*/), //是否为移动终端
          ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
          android: u.indexOf("Android") > -1 || u.indexOf("Linux") > -1, //android终端或者uc浏览器
          iPhone: u.indexOf("iPhone") > -1, //是否为iPhone或者QQHD浏览器
          iPad: u.indexOf("iPad") > -1, //是否iPad
          webApp: u.indexOf("Safari") == -1, //是否web应该程序，没有头部与底部
        };
      })(),
      language: (navigator.browserLanguage || navigator.language).toLowerCase(),
    };
    //如果是移动端就进行这里
    if (
      browser.versions.mobile ||
      browser.versions.ios ||
      browser.versions.android ||
      browser.versions.iPhone ||
      browser.versions.iPad
    ) {
      window.addEventListener(
        "onorientationchange" in window ? "orientationchange" : "resize",
        () => {
          if (window.orientation === 180 || window.orientation === 0) {
            this.isMobile = true;
          }
          if (window.orientation === 90 || window.orientation === -90) {
          }
        },
        false
      );
    } else {
      //pc端进入这里
    }
    this.get()
  },
  methods: {
    createXMLHTTPRequest() {
      //1.创建XMLHttpRequest对象
      //这是XMLHttpReuquest对象无部使用中最复杂的一步
      //需要针对IE和其他类型的浏览器建立这个对象的不同方式写不同的代码
      var xmlHttpRequest;
      if (window.XMLHttpRequest) {
        //针对FireFox，Mozillar，Opera，Safari，IE7，IE8
        xmlHttpRequest = new XMLHttpRequest();
        //针对某些特定版本的mozillar浏览器的BUG进行修正
        if (xmlHttpRequest.overrideMimeType) {
          xmlHttpRequest.overrideMimeType("text/xml");
        }
      } else if (window.ActiveXObject) {
        //针对IE6，IE5.5，IE5
        //两个可以用于创建XMLHTTPRequest对象的控件名称，保存在一个js的数组中
        //排在前面的版本较新
        var activexName = ["MSXML2.XMLHTTP", "Microsoft.XMLHTTP"];
        for (var i = 0; i < activexName.length; i++) {
          try {
            //取出一个控件名进行创建，如果创建成功就终止循环
            //如果创建失败，回抛出异常，然后可以继续循环，继续尝试创建
            xmlHttpRequest = new ActiveXObject(activexName[i]);
            if (xmlHttpRequest) {
              break;
            }
          } catch (e) {}
        }
      }
      return xmlHttpRequest;
    },
    get() {
      var req = this.createXMLHTTPRequest();
      if (req) {
        req.open("GET", "./json.js", true);
        req.onreadystatechange = function () {
          if (req.readyState == 4) {
            if (req.status == 200) {
              this.imgSmallArr= req.obj.imgSmallArr;
              this.imgArr= req.obj.imgArr;
            } else {
              alert("error");
            }
          }
        };
        req.send(null);
      }
    },
  },
};
</script>

<style>
html,
body,
#app {
  padding: 0;
  margin: 0;
  width: 100%;
  height: 100%;
  background: black;
}
div,
iframe,
table,
h1,
h2,
h3,
h4,
h5,
h6,
p,
blockquote,
a,
pre,
address,
big,
cite,
code,
del,
em,
font,
img,
ins,
small,
strong,
var,
b,
u,
center,
dl,
dt,
dd,
ol,
ul,
li,
fieldset,
form,
label,
legend {
  margin: 0;
  padding: 0;
  text-decoration: none;
  list-style: none;
}
.headerBg {
  width: 100%;
  height: 200px;
  background: url("../src/assets/bg.png");
}

.popBg {
  height: 100%;
  width: 100%;
  position: fixed;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.name {
  color: white;
  font-size: 20px;
  margin-top: 20px;
}
.img {
  -webkit-transform: rotate(180deg);
  -moz-transform: rotate(180deg);
  -o-transform: rotate(180deg);
  -ms-transform: rotate(180deg);
  transform: rotate(180deg);
  /*动画属性*/
  animation: myAnimation 1s;
  animation-iteration-count: infinite;
  animation-timing-function: linear;
}

/* 定义动画如何执行 */
@keyframes myAnimation {
  /* 写法1 这种写法只能定义开始和结束时的动画 */
  form {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(270deg);
  }
}

/* 超小屏幕（手机，小于 768px） */
@media (max-width: 768px) {
  .img {
    height: 200px;
    width: auto;
  }
  .content {
    text-align: center;
    width: 400px;
    margin: 0 auto;
    margin-top: 60px;
  }
  .contentImgFather {
    display: inline-block;
    width: 200px;
    height: 200px;
    margin: 30px;
  }
  .contentImg {
    width: 200px;
    height: 200px;
  }
  .smallImgFather {
    display: inline-block;
    width: 200px;
    height: 200px;
    margin: 30px;
  }
  .smallImg {
    width: 70px;
    height: 70px;
    margin: 8px;
  }
}

/* 小屏幕（平板，大于等于 768px） */
@media (min-width: 768px) {
  .img {
    height: 200px;
    width: auto;
  }
  .content {
    text-align: center;
    width: 600px;
    margin: 0 auto;
    margin-top: 60px;
  }
  .contentImgFather {
    display: inline-block;
    width: 200px;
    height: 200px;
    margin: 30px;
  }
  .contentImg {
    width: 200px;
    height: 200px;
  }
  .smallImgFather {
    display: inline-block;
    width: 200px;
    height: 200px;
    margin: 30px;
  }
  .smallImg {
    width: 70px;
    height: 70px;
    margin: 8px;
  }
}

/* 中等屏幕（桌面显示器，大于等于 992px） */
@media (min-width: 992px）) {
  .img {
    height: 200px;
    width: auto;
  }
  .content {
    text-align: center;
    width: 800px;
    margin: 0 auto;
    margin-top: 60px;
  }
  .contentImgFather {
    display: inline-block;
    width: 200px;
    height: 200px;
    margin: 30px;
  }
  .contentImg {
    width: 200px;
    height: 200px;
  }
  .smallImgFather {
    display: inline-block;
    width: 200px;
    height: 200px;
    margin: 30px;
  }
  .smallImg {
    width: 70px;
    height: 70px;
    margin: 8px;
  }
}

/* 大屏幕（大桌面显示器，大于等于 1200px） */
@media (min-width: 1200px) {
  .img {
    height: 200px;
    width: auto;
  }
  .content {
    text-align: center;
    width: 800px;
    margin: 0 auto;
    margin-top: 60px;
  }
  .contentImgFather {
    display: inline-block;
    width: 200px;
    height: 200px;
    margin: 30px;
  }
  .contentImg {
    width: 200px;
    height: 200px;
  }
  .smallImgFather {
    display: inline-block;
    width: 200px;
    height: 200px;
    margin: 30px;
  }
  .smallImg {
    width: 70px;
    height: 70px;
    margin: 8px;
  }
}
</style>
