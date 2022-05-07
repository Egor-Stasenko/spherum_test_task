<template>
    <div v-if="visible" v-on:animationend="reset" :style="'background-color: rgb('+r+','+gb+','+gb+');'" class="notification">
        <span class="text">
            <slot></slot>
        </span>
    </div>
</template>

<script>
export default {
    data(){
        return {
            visible: false,
            r: 200,
            gb: 50
        }
    },
    methods: {
        reset(){
            this.visible = false;
            this.r = 200;
            this.gb = 50;
        },
        notify(){
            if(!this.visible){
                this.visible = true;
            }else{
                this.r += 20;
                this.gb -= 10;
            }
        }
    }
}
</script>
<style>
    .notification{
        height: 32px;
        width: 250px;
        transform-origin: right;
        display: flex;
        justify-content: center;
        align-items: center;
        transition-duration: 0.15s;
        animation: notify 1.5s ease-in-out forwards;
    }
    @keyframes notify{
        0%     { transform: scaleX(0);  }
        15.38% { transform: scaleX(1);  }
        20%    { transform: scale(1.1); }
        24.6%  { transform: scale(1);   }
        84.6%  { transform: scaleX(1);  }
        100%   { transform: scaleX(0);  }
    }

    .text{
        animation: exit 2s ease-in-out forwards;
    }
    @keyframes exit{
        0%   { opacity: 1; }
        55%  { opacity: 1; }
        65%  { opacity: 0; }
        100% { opacity: 0; }
    }
</style>