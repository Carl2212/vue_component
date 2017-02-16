<template>
    <div class="drawer-box"  :style="{minHeight : iheight,transform : 'translateX('+trsX+')'}">
        <div @onshow = "onshow()" ref="navModal" class="fl leftwidth" :style="{height : iheight}">
            <button class="nav-root nav-icon" @click="drawers()">
                <span class="glyphicon glyphicon-menu"></span>
            </button>
            <div class="nav-main">
                <slot name="nav"></slot>
            </div>
        </div>
        <div class="contrainer fl" :style="{width : iWidth}">
            <slot></slot>
            <div id="md" class="md" :class="{modal:ismodal}" :style="{height : iheight}"></div>
        </div>
    </div>
</template>

<script>
    let iheight = window.innerHeight ||document.documentElement.clientHeight ;
    let iwidth = window.innerWidth || document.documentElement.clientWidth;
    export default {
        name: 'drawer',
        data () {
            return {
                ismodal : false,
                iheight : iheight+'px',
                iWidth : iwidth+'px',
                initx : 0,
                TS : 0,
                trsX : '-300px',
                targetW : 0,

            }
        },
        mounted() {
            var me = this;
            //监听全局 还原初始状态
            var elem = document.getElementsByClassName('drawer-box')[0];
            elem.addEventListener('click',function(e){
                if(e.target.id=='md'){
                    setTimeout(function(){
                        me.onhide();
                    },500);
                    me.trsX = '-'+me.targetW+'px';
                }
            },false);
            //监听滑动事件
            elem.addEventListener('touchstart',this._touchstart,false);
            elem.addEventListener('touchmove',this._touchmove,false);
            elem.addEventListener('touchend',this._touchend,false);

            //获取nav宽度 设置drawer组件移动位置
            this.targetW = document.getElementsByClassName('nav-main')[0].offsetWidth;
            this.trsX = '-'+this.targetW+'px';
        },
        methods : {
            onshow() {
                this.ismodal =true;
                this.trsX = 0;
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
                    let distance = (this.targetW - x) > 0 ?(this.targetW - x) : 0;
                    this.trsX = -distance+'px';
                }
            },
            _touchend(e) {
                if(this.initx > 0) {
                    let w = this.trsX.substring(0,this.trsX.length-3)/1;
                    if(this.targetW + w > this.targetW/2) {
                        this.trsX = 0;
                    }else{
                        this.trsX = '-'+this.targetW+'px';
                        this.onhide();
                    }
                    this.initx = 0;
                }
            }
        }
    }
</script>

<style>
    @import "../static/font-style.css";
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
        display: block;
    }

    a {
        color: #42b983;
    }
    button{
        padding : 0;
        margin : 0;
        background :none;
        border : none;
    }
    .drawer-box {
        position: relative;
        border : 1px solid transparent;
        white-space: nowrap;
        transition: transform .5s linear;
        font-size: 0;
        -webkit-text-size-adjust: none;
    }

    .drawer-box .fl{
        display: inline-block;
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        font-size: 15px;
        vertical-align:top;
    }

    .drawer-box .leftwidth{
        width: auto;
    }

    .contrainer{
        margin-top : 0;
        text-align: center;
        transition: transform .5s linear;
        width: 500px;
        position: relative;
    }
    .contrainer .content{
        margin: 1em .5em;
    }

    .nav-icon{
        position: fixed;
        top : 1.25em;
        left : 310px;
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
    .nav-main{
        position: relative;
        top : 0;
        left :0 ;
        width: 300px;
        height: 100%;
        background : #303030;
    }
</style>
