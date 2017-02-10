<template>
  <div id="app" :class="{modal : ismodal}" :style="{height :iheight}">
    <div class="header-bar glyphicon"><p>Vue抽屉菜单效果组件</p></div>
    <drawer @onshow = "onshow()">
      <span class="nav-icon glyphicon glyphicon-menu" slot="nav-icon"></span>

      <ul>
        <li class="nav-li"><a href="">xxxxxx</a> </li>
      </ul>

    </drawer>
    <div class="contrainer">
      <h1>{{ title }}</h1>
      <h2>——{{author}}——</h2>
      <div class="content">我是内容</div>
    </div>

  </div>
</template>

<script>
  import drawer from './component/drawer.vue';
  var iheight = window.innerHeight ;
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
      iheight : 'auto',
    }
  },
  methods : {
    onshow() {
      this.ismodal =true;
      this.iheight = iheight;
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
    margin-top : 2em;
    text-align: center;
  }
  .contrainer .content{
    margin: 1em .5em;
  }
  button{
    background :none;
    border : none;
  }
  .nav-icon{
    position: absolute;
    top : 1em;
    left : .5em;
    font-size: 1.5em;
    color: #fff;
  }

  .modal {
    height: 100%;
    background : rgba(100,100,100,.5);
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
