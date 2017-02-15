<template>
  <div id="app"  :style="{minHeight : iheight}">
    <div class="contrainer" >
      <div class="header-bar"><p>Vue抽屉菜单效果组件</p></div>
      <h1>{{ title }}</h1>
      <h2>——{{author}}——</h2>
      <div class="content">我是内容</div>
    </div>
    <div id="md" class="md" :class="{modal : ismodal}" :style="{height :iheight}">
    </div>
    <drawer @onshow = "onshow()" ref="navModal">
      <span class="nav-icon glyphicon glyphicon-menu" slot="nav-icon"></span>
      <ul>
        <li class="nav-li"><a href="">xxxxxx</a> </li>
      </ul>
    </drawer>
  </div>
</template>

<script>
  import drawer from './component/drawer.vue';
  let iheight = window.innerHeight ||document.documentElement.clientHeight ;
export default {
  name: 'app',
  components : {
    drawer : drawer
  },
  data () {
    return {
      title: 'A Vue.js demo for Drawer',
      author : 'wxh',
      ismodal : false,
      iheight : iheight+'px',
      initx : 0,
      TS : 0
    }
  },
  mounted() {
    var me = this;
    var elem = document.getElementById('app');
    elem.addEventListener('click',function(e){
      if(e.target.id=='md'){
        setTimeout(function(){
          me.onhide();
        },500);
        me.$refs.navModal.hidedrawers();
      }
    },false);
    elem.addEventListener('touchstart',this._touchstart,false);
    elem.addEventListener('touchmove',this._touchmove,false);
    elem.addEventListener('touchend',this._touchend,false);
  },
  methods : {
    onshow() {
      this.ismodal =true;
    },
    onhide() {
      this.ismodal = false;
    },
    _touchstart(e) {
      if(e.touches.length > 0 && e.touches[0].clientX > 0 && e.touches[0].clientX < 50) {
        this.ismodal =true;
        this.initx = e.touches[0].clientX;
      }
    },
    _touchmove(e) {
      e.preventDefault();
      if(this.initx > 0 && e.touches.length > 0 && e.touches[0].clientX > this.initx ) {
        //相对起始位置右移
        var x = e.touches[0].clientX - this.initx;
        this.$refs.navModal.drawermove(x);
      }
    },
    _touchend(e) {
      if(this.initx > 0) {
        this.$refs.navModal.drawerend();
        this.initx = 0;
      }
    }
  }
}
</script>

<style>
  @import "./static/font-style.css";

  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    position: relative;
    border : 1px solid transparent;
  }
  .header-bar{
    height: 3em;
    width: 100%;
    background :#0288bb;
    font-size: 1.5em;
  }
  .header-bar p {
    color:#fff;
    margin: 0 auto;
    height: 3em;
    line-height: 3em;
    text-align: center;
  }
  .contrainer{
    margin-top : 0;
    text-align: center;
    transition: transform .5s linear;
  }
  .contrainer .content{
    margin: 1em .5em;
  }

  .nav-icon{
    position: fixed;
    top : 1.25em;
    left : .5em;
    font-size: 1.5em;
    color: #fff;
    z-index: 99;
  }
  .md{
    position: absolute;
    z-index : 99;
    top : 0 ;
    left : 0;
    width: 100%;
    background : rgba(100,100,100,.5);
    display: none;
  }
  .modal {
    display: block;
  }
  body,div,h1,h2,ul,li,p{
    padding : 0;
    margin : 0;
  }
  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
</style>
