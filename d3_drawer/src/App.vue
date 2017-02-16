<template>
  <div id="app">
      <drawer>
        <ul slot="nav">
          <li>xxxxxxxxxxxxxxxxx</li>
        </ul>

        <div class="header-bar"><p>Vue抽屉菜单效果组件</p></div>
        <h1>{{ title }}</h1>
        <h2>——{{author}}——</h2>
        <div class="content">我是内容</div>

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
</style>
