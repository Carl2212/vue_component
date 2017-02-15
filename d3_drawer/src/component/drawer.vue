<template>
    <div>
        <button class="nav-root" @click="drawers()">
            <slot name="nav-icon"></slot>
        </button>
        <div class="nav-main" :style="{transform : 'translateX('+trsX+')'}">
            <slot></slot>
        </div>
    </div>
</template>

<script>
    export default{
        data(){
            return {
                trsX : '-100%',
                targetW : 0,
            }
        },
        methods : {
            drawers() {
                this.trsX = 0;
                this.targetW = document.getElementsByClassName('nav-main')[0].offsetWidth;
                this.$emit('onshow');
            },
            hidedrawers() {
                this.trsX = '-100%';
            },
            drawermove(x) {
                this.targetW = document.getElementsByClassName('nav-main')[0].offsetWidth;
                let distance = (this.targetW - x) > 0 ?(this.targetW - x) : 0;
                this.trsX = -distance+'px';
            },
            drawerend() {
                let w = this.trsX.substring(0,this.trsX.length-3)/1;
                if(this.targetW + w > this.targetW/2) {
                    this.trsX = 0;
                }else{
                    this.trsX = '-100%';
                    this.$parent.onhide();
                }
            }
        }
    }
</script>
<style>
    button{
        padding : 0;
        margin : 0;
        background :none;
        border : none;
    }
    .nav-main{
        position: fixed;
        z-index:100;
        top : 0;
        left :0 ;
        width: 50%;
        height: 100%;
        background : #fff;
        transition: transform .5s linear;
    }
</style>
