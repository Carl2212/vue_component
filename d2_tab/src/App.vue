<template>
  <div id="app">
    <h1>{{ title }}</h1>
    <h2>——wxh——</h2>
    <tab :tabData = "tabTitle" @loadcontent="loadcontent">
      <div class="tab-content-group">
        <div class="tab-content" :style="{'width' : width+'px'}" v-for="item in tabTitle">
          {{item.content}}
          <div class="loading" v-if="!item.content">...........</div>
        </div>
      </div>
    </tab>
  </div>
</template>

<script>
  import tab from "./component/tab.vue";
  let tabTitle = [
    {title : 'Test1',content : 'component1',path : './component1',isactive : true},
    {title : 'Test2',content : '',path : './component2',isactive : false},
    {title : 'Test3',content : 'component3',path : './component3',isactive : false},
    {title : 'Test4',content : 'component4',path : './component4',isactive : false}
  ];
  let W = window.innerWidth || document.docuemntElement.clientWidth;
  let H = window.innerHeight|| document.docuemntElement.clientHeight;
  export default {
    name: 'app',
    components : {
      tab : tab
    },
    data () {
      return {
        title: 'A Vue.js demo for tab',
        tabTitle : tabTitle,
        width : W
      }
    },
    methods : {
      loadcontent(index) {
        console.log(index);
        //循环判断当前组件是否在可视区域范围内
        let item = this.$('.tab-content')[index];
        let load = this.children(item , '.loading');
        let self = this;
        if(load && load.length >0){//内容为空
            setTimeout(function(){
              self.tabTitle[index].content = 'component'+(index+1);
            },3000);
        }
      },
      $(elem,parent) {
        parent = parent ? parent : document;
        var res = [];
        if(parent.querySelectorAll) {
          res = parent.querySelectorAll(elem);
        }else {
          var styleobj = parent.styleSheets[0] || parent.createStyleSheet();
          styleobj.addRule(elem , 'wxhstyle : true');
          for(var i = 0 ; i< parent.all.length ; i++) {
            if(parent.all[i].currentStyle.wxhstyle) {
              res.push(parent.all[i]);
            }
          }
          styleobj.removeRule('wxhstyle');
        }
        res = this.converListToArray(res);
        return res;
      },
      converListToArray(nodelist) {
        var res = [];
        try{
          res = Array.prototype.splice.call(nodelist);
        }catch(error){
          for(var i= 0 ; i< nodelist.length ;i++) {
            res.push(nodelist[i]);
          }
        }
        return res;
      },
      children(elem,selector) {
        var children = elem.childNodes;
        var res = [];
        for(var i = 0 ; i< children.length ; i++) {
          if(children[i].nodeType == 1 && children[i] != elem) {
            if(selector) {
              console.log(selector,elem);
              var child = this.$(selector , elem);
              console.log(child);
              //js数组合并的方法 var c = a.concat(b); a.push.apply(a,b)
              res.push.apply(res , child);
            }
          }
        }
        console.log(res);
        return res;
      }
    }
  }
</script>

<style>
  body , div,ul ,li {
    padding : 0;
    margin : 0;
  }
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
}

li {
  display: inline-block;
}

a {
  color: #42b983;
}
.tab-content-group{
    width: auto;
    white-space: nowrap;
 }
.tab-content-group:after{
  display: block;
  content : " ";
  clear: both;
}
.tab-content-group .tab-content{
  display: inline-block;
  background : #e0e0e0;
  min-height: 300px;
  font-size: 20px;
  padding-top:100px;
  text-align: center;
}
</style>
