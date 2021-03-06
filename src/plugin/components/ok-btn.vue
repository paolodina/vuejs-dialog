<template>
    <button v-if="enabled" :class="['btn', options.btnClsOk, {'dg-btn--loading': loading}, {'dg-pull-right': !options.reverse}]"
            @click.prevent="proceed()" ref="btn" :disabled="is_disabled">
        <span class="dg-btn-content">
            <slot></slot>
            <span v-if="soft_confirm">({{ clicks_remaining }})</span>
        </span>
        <span is="btn-loader" v-if="loading"></span>
    </button>
</template>

<script>
    import BtnLoader from './btn-loader.vue'
    import {DIALOG_TYPES, CONFIRM_TYPES, ANIMATION_TYPES} from '../js/constants'

    export default {
        data(){
            return {
                clicks_count: 0
            }
        },
        props: {
            btnClsOk: '',
            enabled: {
                required: false,
                type: Boolean,
                'default': true
            },
            options: {
                required: true,
                type: Object
            },
            focus: {
                required: false,
                type: Boolean,
                'default': false
            },
            loading: {
                required: false,
                type: Boolean,
                'default': false
            }
        },
        mounted(){
            this.focus && this.$refs.btn.focus()
        },
        computed: {
            soft_confirm(){
                return (this.options.type === CONFIRM_TYPES.SOFT)
            },
            hard_confirm(){
                return (this.options.type === CONFIRM_TYPES.HARD)
            },
            is_disabled(){
                return (this.$parent.okBtnDisabled)
            },
            clicks_remaining(){
                return Math.max((this.options.clicksCount - this.clicks_count), 0)
            }
        },
        methods: {
            proceed(){
                if(!this.is_disabled && this.validateProceed()){
                    this.$emit('click')
                }
            },
            validateProceed(){
                switch (this.options.type){
                    case CONFIRM_TYPES.SOFT:
                        this.clicks_count++
                        return (this.clicks_count >= this.options.clicksCount)
                        break;
                    case CONFIRM_TYPES.BASIC:
                    default:
                        return true;
                }
            },
        },
        components: {BtnLoader}
    }
</script>