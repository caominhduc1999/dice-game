<template>
    <div class="wrapper-popup" v-bind:class="getClassPopup">
        <div class="rule">
            <h3>Rule</h3>
            <p>Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the
                industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and
                scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap
                into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the
                release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing
                software like Aldus PageMaker including versions of Lorem Ipsum</p>
            <button
                v-on:click="confirm"
            >Understand</button>
        </div>
    </div>
</template>

<script>
    export default {
        name: "PopupRule",
        props:{
            isOpenPopup : {
                type: Boolean, default: false
            }
        },
        methods:{
            confirm(){
                this.$emit('handleConfirm')
            }
        },
        computed:{
            getClassPopup: function () {
                return {
                    'open-popup': this.isOpenPopup
                }
            }
        }
    }
</script>

<style scoped>
    .wrapper-popup {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0, 0, 0, 0.4);
        opacity: 0;
        visibility: hidden;
    }

    .wrapper-popup.open-popup{
        opacity: 1;
        visibility: visible;
    }

    .rule {
        background-color: #fff;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%) scale(1.2);
        position: absolute;
        width: 300px;
    }

    .open-popup .rule{
        transform: translate(-50%, -50%) scale(1);
    }
</style>
