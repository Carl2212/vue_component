<template>
  <div id="app">
      <drawer>
        <ul slot="nav" class="nav-contrainer">
          <li class="nav-title"><a href="#"><strong><span class="glyphicon glyphicon-th-large"></span> 菜单</strong></a></li>
          <li class="nav-title"><a href="#"><span class="glyphicon glyphicon-book"></span> 上滑/下滑刷新</a></li>
          <li class="nav-title"><a href="#"><span class="glyphicon glyphicon-link"></span> 选择标签切换</a></li>
          <li class="nav-title"><a href="#"><span class="glyphicon glyphicon-paperclip"></span> Vue抽屉菜单组件</a></li>
          <li class="nav-title"><a href="#"><span class="glyphicon glyphicon-pushpin"></span> 提示信息组件</a></li>
        </ul>

        <div class="header-bar"><p>Vue抽屉菜单效果组件</p></div>
        <h1>{{ title }}</h1>
        <h2>——{{author}}——</h2>
        <div class="content">
          <p>我是内容</p>
          <p>我是内容</p>
          <p>我是内容</p>
          <p>我是内容</p>
          <p>我是内容</p>
          <p>我是内容</p>
          <p>我是内容</p>
          <p>我是内容</p>
          <p>我是内容</p>
          <p>我是内容</p>
          <p>我是内容</p>
          <p>我是内容</p>
        </div>

      </drawer>
  </div>
</template>

<script>
  import drawer from './component/drawer.vue';
export default {
  name: 'app',
  components : {
    drawer : drawer
  },
  data () {
    return {
      title: 'A Vue.js demo for Drawer',
      author : 'wxh',
    }
  },
  methods : {

  }
}
</script>

<style>
  @import "./static/font-style.css";
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
  .content p {
    height: 3em;
    line-height: 3em;
  }
  .nav-contrainer{
    width: 100%;
    text-align: left
  }
  .nav-contrainer .nav-title{
    width: 100%;
    font-size: 1.5em;
    height : 4em;
    line-height:4em;
    border-bottom: 1px solid #888;
  }
  .nav-contrainer .nav-title a{
    margin-left: 1em;
    color : #fff;
    text-decoration : none;
  }
  .nav-contrainer .nav-title strong {

  }
</style>
