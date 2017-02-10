<template>
    <div>
        <button class="nav-root" @click="drawers()">
            <slot name="nav-icon"></slot>
        </button>
        <div class="nav-main" :class="{show : isshow,hide : !isshow}">
            <slot></slot>
        </div>
    </div>
</template>

<script>
    export default{
        data(){
            return {
                isshow : false,
            }
        },
        methods : {
            drawers() {
                this.isshow = true;

                this.$emit('onshow');
            }
        }
    }
</script>
<style>
    .show {
        transform : translateX(0);
        transition: transform .5s linear;
    }
    .hide{
        transform : translateX(-100%);

    }
    .nav-main{
        position: fixed;
        top : 0;
        left :0 ;
        width: 50%;
        height: 100%;
        background : #fff;
    }
</style>
