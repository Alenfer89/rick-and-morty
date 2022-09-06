<template>
    <!-- <div class="modal-dialog modal-dialog-centered"> -->
    <div class="modal-background"
    @click='closeModal'
    >
        <h1>
            {{ character.name }}
        </h1>
    </div>
</template>

<script>
export default {
    name: 'ModalCard',
    props: ['character'],
    methods: {
        closeModal(){
            this.$emit('close', false)
        }
    }
}
</script>

<style lang='scss' scoped>
.modal-background{
    position: fixed;
    z-index: 3;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center
}
</style>