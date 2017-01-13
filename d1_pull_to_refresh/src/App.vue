<template>
  <div id="app">
    <pulltorefresh :style="{'height':iHeight+'px'}" class="page" @on-pullup="pullup" @on-pulldown="pulldown">
      <ul class="content-box"  >
        <li clas="list-txt" v-for="item in items" >
          {{item}}
        </li>
      </ul>
    </pulltorefresh>
  </div>
</template>

<script>
  import PullToRefresh from './components/pull_to_refresh.vue';
  let itemsP = [];
  for(var i = 1 ; i<18 ;i ++) {
    itemsP.push('item'+i);
  }
  let H = document.body.scrollHeight;
  console.log(H);
  export default {
    name: 'app',
    components : {
      pulltorefresh : PullToRefresh
    },
    data () {
      return {
        msg: 'Welcome to Your Vue.js App',
        items : itemsP,
        iHeight : document.body.scrollHeight-20//保证下拉框距离底部有一定的小距离
      }
    },
    methods: {
      pullup(callback) {
        var me = this;
        setTimeout(function(){
          var num = me.RandomNumBoth(1000,9999);
          for(var i = 1 ; i<= 5 ;i ++) {
            me.items.push('item'+num+i);
          }
          callback && callback();
        },2000);
      },
      pulldown(callback){
        var me = this;
        setTimeout(function(){
          var num = me.RandomNumBoth(1000,9999);
          for(var i = 1 ; i<= 5 ;i ++) {
            me.items.unshift('item'+num+i);
          }
          callback && callback();
        },2000);
      },
      RandomNumBoth(Min,Max){
        var Range = Max - Min;
        var Rand = Math.random();
        var num = Min + Math.round(Rand * Range); //四舍五入
        return num;
      }
    }
  }
</script>

<style>
  body{
    background-color:#eee;
    overflow-y : hidden;
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
    display: block;
    height : 3em;
    line-height:3em;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
  #app {
    text-align: center;
  }
  .page{
    position: relative;
    border: 1px solid transparent;/*防止边界融合 导致scroll的上边距移到外层 */
  }
  .content-box{
    margin-bottom : 1em;
  }
  /*scrollbar start*/
  .iScrollVerticalScrollbar {
    position: absolute;
    z-index: 9999;
    width: 2px;
    bottom: 2px;
    top: 2px;
    right: 2px;
    overflow: hidden;
  }

  .iScrollVerticalScrollbar.iScrollBothScrollbars {
    bottom: 18px;
  }

  .iScrollIndicator {
    position: absolute;
    background: #999;
    border-radius: 6px;
    opacity: .8;
  }

  .iScrollVerticalScrollbar .iScrollIndicator {
    width: 100%;
    background: #999;
  }
  /*scrollbar end*/
</style>
