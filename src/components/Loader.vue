<template>
    <div id="loader">
        <span class="spinner-border text-success" role="status">
            <span class="sr-only">Loading...</span>
        </span>
    </div>
</template>

<script>
export default {
    name: "MainLoader",
};
</script>

/*
<style lang="scss" scoped>
    #loader {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0, 0, 0, 0.5);
        display: flex;
        justify-content: center;
        align-items: center;
        z-index: 3;
        .spinner-border {
            width: 150px;
            height: 150px;
        }
    }
</style>