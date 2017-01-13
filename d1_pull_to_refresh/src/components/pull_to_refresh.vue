<template>
    <div :id="elmId" class="pull-contrainer">
        <div class="scroller">
            <div class="pull-down-box" :class="{hide : ispulldownhide}" :style="{'margin-top':'-'+pullDownDiff+'px'}">
                <span class="down" :class="{active : isdownactive , loading : isdownloading}"></span>
                <span class="down-txt">{{pull_down_txt}}</span>
            </div>
            <slot></slot>
            <div class="pull-up-box" :class="{hide : ispulluphide}">
                <span class="up" :class="{active : isupactive , loading : isuploading}"></span>
                <span class="up-txt">{{pull_up_txt}}</span>
            </div>
        </div>
    </div>
</template>
<script type="es6">
    import IScroll from "../util/iscroll-probe";
    const PULLUPTXT="下拉刷新";
    const PULLTOLOADING="释放加载";
    const LOADING="加载中...";
    const PULLDOWNTXT="上拉刷新";

    const PULLHEIGHT = "100";
    export default{
        data(){
            return {
                elmId : 'bajianscroll',
                pull_down_txt: PULLTOLOADING,//动画效果之字体
                pull_up_txt: PULLTOLOADING,

                ispulldownhide : true,//动画效果之总元素
                ispulluphide : true,

                direction : 0,//拉动方向 0 为不动  -1 为向下 1 为向上

                isdownactive : false,//箭头是否激活状态
                isupactive : false,

                isdownloading : false,//loading是否激活状态
                isuploading : false,

                pullDownDiff : -100,//下拉时margin-top初始状态

                isloadingdata : false,//是否加载数据的最终状态
            }
        },
        mounted(){
            this.scroll = new IScroll('#'+this.elmId, {
                probeType: 2,//probeType：1对性能没有影响。在滚动事件被触发时，滚动轴是不是忙着做它的东西。probeType：2总执行滚动，除了势头，反弹过程中的事件。这类似于原生的onscroll事件。probeType：3发出的滚动事件与到的像素精度。注意，滚动被迫requestAnimationFrame（即：useTransition：假）。
                scrollbars: 'custom',//有滚动条
                mouseWheel: true,//允许滑轮滚动
                fadeScrollbars: true,//滚动时显示滚动条，默认影藏，并且是淡出淡入效果
                bounce:true,//边界反弹
                listenX: false,
                interactiveScrollbars:true,//滚动条可以拖动
                shrinkScrollbars:'scale',// 当滚动边界之外的滚动条是由少量的收缩。'clip' or 'scale'.
                click: true ,// 允许点击事件
                momentum:true,// 允许有惯性滑动
                preventDefaultException: { tagName: /^(INPUT|TEXTAREA|BUTTON|SELECT|IMG)$/ }
            });
            this.scroll.refresh();
            this.scroll.on('scroll',this._onmove);
            this.scroll.on('scrollEnd',this._onmoveEnd);
        },
        methods : {
            _onmove() {
                //判断下拉小于PULLHEIGHT ,下拉大于PULLHEIGHT ,上拉 小于MaxScrollY+PULLHEIGHT ,上拉大于MaxScrollY + PULLHEIGHT
                if(this.direction == -1 && this.scroll.y < PULLHEIGHT/2) {
                    //1 样式文字图标变化
                    this.pull_down_txt = PULLUPTXT;
                    this.isdownactive = false;
                    //2 下拉上拉框显示隐藏
                    this.ispulluphide = true;
                    this.ispulldownhide = false;
                    //3 下拉位置margin-top移动
                    this.pullDownDiff = PULLHEIGHT - this.scroll.y;
                    //4 是否刷数据的最终状态
                    this.isloadingdata = false;
                }else if(this.direction == -1 && this.scroll.y > PULLHEIGHT/2) {
                    //1 样式文字图标变化
                    this.pull_down_txt = PULLTOLOADING;
                    this.isdownactive = true;
                    //3 下拉位置margin-top移动
                    this.pullDownDiff = PULLHEIGHT - this.scroll.y;
                    this.isloadingdata = true;
                }else if(this.direction == 1 && -this.scroll.y < PULLHEIGHT/1 -this.scroll.maxScrollY && !this.isupactive) {
                    //1 样式文字图标变化
                    this.pull_up_txt = PULLDOWNTXT;
                    //2 下拉上拉框显示隐藏
                    this.ispulldownhide = true;
                    this.ispulluphide = false;
                    //4 是否刷数据的最终状态
                    this.isloadingdata = false;
                }else if(this.direction == 1 && -this.scroll.y >= PULLHEIGHT/1 -this.scroll.maxScrollY ){
                    //1 样式文字图标变化
                    this.pull_up_txt = PULLTOLOADING;
                    this.isupactive = true;

                    //4 是否刷数据的最终状态
                    this.isloadingdata = true;

                    //3 上拉触发 滚动高度 保证释放时pull-up-box 不会下滑消失
                    this.scroll.refresh();
                }
                this.direction = this.direction || this.scroll.directionY;
            },
            _onmoveEnd() {
                if(this.direction == -1 && this.isdownactive && this.isloadingdata) {
                    //1 样式文字图标变化
                    this.isdownloading = true;
                    this.pull_down_txt = LOADING;
                    //5 刷数据 回调恢复状态
                    this.$emit('on-pulldown',this.reset);
                }else if(this.direction == 1 && this.isupactive && this.isloadingdata) {
                    //1 样式文字图标变化
                    this.isuploading = true;
                    this.pull_up_txt = LOADING;
                    //5 刷数据 回调恢复状态
                    this.$emit('on-pullup',this.reset);
                }
            },
            //重置原始状态
            reset() {
                if(this.direction == -1) {
                    this.pull_down_txt= PULLUPTXT;
                    this.ispulldownhide = true;
                    this.isdownactive = false;
                    this.isdownloading = false;
                    this.pullDownDiff = -100;
                }else if (this.direction == 1) {
                    this.pull_up_txt= PULLDOWNTXT;
                    this.ispulluphide = true;
                    this.isuploading = false;
                    this.isupactive = false;
                }
                //this.scroll.refresh();
                var me = this;
                setTimeout(()=>{
                    this.scroll.refresh();
                },0)
                this.direction = 0 ;
                this.isloadingdata = false;
            }
        }
    }
</script>
<style>
    .scroller{
        min-height: 101%;/*修正内容高度不够无法scroll*/
        -webkit-tap-highlight-color:rgba(0,0,0,0);
        height :auto;
    }
    .pull-up-box,.pull-down-box{
        position: relative;
        height : 50px;
        width: 99%;
        text-align: center;
        padding-top : 50px;
        line-height: 50px;
        border : 1px solid #eee;
        margin-top : -100px;
    }
    .pull-up-box.hide ,  .pull-down-box.hide{
        display: none;
    }
    .pull-up-box{
        margin-top : 0;
        height : 100px;
        padding-top : 0;
        line-height: 100px;
    }
     .down , .up {
        display: block;
        position: absolute;
        top : 75%;
        left:50%;
        margin-left:-3.4em;
        margin-top: -.7em;
        width : 1.4em;
        height:1.4em;
        background: url(../assets/pull-icon@2x.png) 0 0 no-repeat;
        background-size: 100% 200%;
        transition: all 1s;
    }
    .up{
        top : 50%;
    }
    .down.loading , .up.loading {
        background: url(../assets/pull-icon@2x.png) 0 100% no-repeat;
        background-size: 100% 200%;
    }
    .down.active{
        transform : rotateZ(180deg);
    }
    .up.active{
        transform : rotateZ(180deg);
    }
</style>
