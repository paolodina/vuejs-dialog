<template>
    <button v-if="enabled" :class="['btn', options.btnClsCancel, {'dg-pull-right': options.reverse}]"
            @click.prevent="$emit('click')" ref="btn">
        <slot></slot>
    </button>
</template>

<script>
    export default {
        props: {
            btnClsCancel: '',
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
        }
    }
</script>