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
            var elem = document.getElementsByClassName('scroll-page')[0];
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
                console.log(this.TS);
                this.activeItem = index;
                //发射事件到父组件上
                this.$emit('loadcontent',index);
            },
            _touchStart(e) {
                this.moveX = e.touches[0].clientX;
            },
            _touchMove(e) {
                let x = e.touches[0].clientX - this.moveX;
                this.TS = x -this.distance;
            },
            _touchEnd(e){
                let index = Math.abs(Math.ceil(this.TS/W));
                if(Math.abs(this.TS+this.distance) >= this.W/2) index = index+1;
                this.distance = index * this.W;
                console.log(this.W,this.distance);
                this.tabchange(index);
            }
        }
    }
</script>
