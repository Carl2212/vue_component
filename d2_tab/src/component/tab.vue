<template>
    <div>
        <ul class="tab-title" >
            <li class="tab-title-item" :class="{active : index == activeItem}" :style="{'width' :wd+'px'}" v-for="(item,index) in tabData" @click="content(item,index)">
                {{item.title}}
            </li>
        </ul>
        <div class="content" >
            <div class="scroll-page" :style="{'transform':'translate('+TS+'px)'}">
                <slot></slot>
            </div>
        </div>
    </div>
</template>
<style>
    .active{
        color : #8660d1;
        position: relative;
    }
    .active:after{
        position: absolute;
        bottom:0;
        left: 0;
        display: block;
        height: .3em;
        content :" ";
        width: 100%;
        z-index:99;
        background :  #8660d1;
    }
    .tab-title {
        white-space: nowrap;
    }
    .tab-title .tab-title-item{
        display: inline-block;
        cursor: pointer;
        padding : .5em 0;
        min-width: 40px;
        transition: transform .5s linear;
    }
    .content{
        width: 100%;
        height : auto;
        overflow-x : hidden;
    }
    .scroll-page{
        width: auto;
        transition: transform .5s linear;
    }
</style>
<script>
    const tabTitleItemMinWidth ='50';
    let W = window.innerWidth || document.documentElement.clientWidth;
    export default{
        props : ['tabData'],
        computed : {
            wd : function(){
                let w = W/this.tabData.length;
                if(w <= tabTitleItemMinWidth) {
                    return tabTitleItemMinWidth;
                }
                return w;
            }
        },
        data(){
            return{
                msg:'hello vue',
                TS : 0,
                W : W,
                activeItem : 0,
                moveX : 0,
                moveY : 0,
                distance : 0,
            }
        },
        mounted() {
            var elem = document.getElementsByClassName('content')[0];
            elem.addEventListener('touchstart',this._touchStart , false);
            elem.addEventListener('touchmove',this._touchMove , false);
            elem.addEventListener('touchend',this._touchEnd , false);

        },
        methods : {
            content(item,index) {
                this.tabchange(index);
            },
            tabchange(index) {
                this.TS = -(this.W * index);
                this.distance = -this.TS;
                this.activeItem = index;
                //发射事件到父组件上
                this.$emit('loadcontent',index);
            },
            _touchStart(e) {
                this.moveX = e.touches[0].clientX;
            },
            _touchMove(e) {
                e.preventDefault();
                let x = this.moveX;
                if(e.layerX || e.layerX == 0) {
                    x = event.layerX- this.moveX;
                }else if(e.offsetX || e.offsetX == 0) {
                    x = event.offsetX- this.moveX;
                }else if(e.targetTouches.length == 1) {
                    var touch = e.targetTouches[0];
                    x = touch.screenX- this.moveX;
                }else if(e.touches.length == 1) {
                    var touch = e.touches[0];
                    x = touch.clientX- this.moveX;
                }
                this.TS = x - this.distance;
            },
            _touchEnd(e){
                e.preventDefault();
                //当前释放位置对应的index（不一定是当前索引页，可能已经移动到上一个索引页）
                let index = Math.abs(Math.ceil(this.TS/W));
                let x = this.TS+this.distance;
                if(this.TS > 0 ) {//当前漂移位置超出最左，index为0
                    index = 0;
                }else if((x <= 0 &&  Math.abs(x) >= this.W/2) || (x > 0 &&  Math.abs(x)<= this.W/2)) {
                    //移动往左，相对位移为负，此时index为当前索引，移动往右，相对位移为正，此时index为上一个索引
                    index = index+1;
                }
                //极端判断
                index = (index < 0) ? 0 : (index > this.tabData.length -1 ? this.tabData.length -1 :index);
                this.tabchange(index);
            }
        }
    }
</script>
