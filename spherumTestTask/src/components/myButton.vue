<template>
    <button ref="button" class="button" @click="animateRipple">
        <slot></slot>
        <transition-group>
            <template v-for="(val, i) in ripples">
                <span
                    class="ripple"
                    v-bind:ref="'ripple-' + i"
                    v-bind:key="'ripple' + i"
                    v-if="val.show === true"
                    v-bind:style="{
                        'top': val.y + 'px',
                        'left': val.x + 'px'
                    }"
                    v-on:animationend="rippleEnd(i)">
                </span>
            </template>
        </transition-group>
    </button>
</template>

<script>
export default {
    data(){
        return {
            ripples: []
        }
    },
    methods: {
		animateRipple(e){
			let el  = this.$refs.button;
			let pos = el.getBoundingClientRect();
			
			this.ripples.push({
				x: e.clientX - pos.left,
				y: e.clientY - pos.top,
				show: true
			});
		},
		rippleEnd(i){
			this.ripples[i].show = false;
		}
    }
}
</script>
<style>
    .button{
        margin-top: auto;
        margin-bottom: auto;
        background-color: #181818;
        color: white;
        border-style: solid;
        border-color: white;
        border-width: 1px;
        padding: 7px;
        min-width: 90px;

        position: relative;
        outline: 0;
        overflow: hidden;
        display: inline-block;
        user-select: none;
    }
    .ripple{
        background-color: #ff1ead;
        width: 1rem;
        height: 1rem;
        position: absolute;
        border-radius: 50%;
        transform: translateX(-100%) translateY(-100%);
        mix-blend-mode: screen;
        animation: ripple 2500ms ease-out forwards, fade 1000ms ease-out forwards;
    }
    @keyframes ripple{
        0%   { transform: translate(-100%, -100%); }
        80%  { transform: translate(-100%, -100%) scale(50); }
        100% { transform: translate(-100%, -100%) scale(50); opacity: 0; }
    }
    @keyframes fade{
        0%   { opacity: 1; }
        100% { opacity: 0; }
    }
</style>