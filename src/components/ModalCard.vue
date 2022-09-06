<template>
    <div class="modal-background">
        <div class="card w-25">
            <img :src="character.image" class="card-img-top position-relative" :alt="'Full image of ' + character.name">
            <button type="button" class="btn-close"
            @click.prevent='closeModal'></button>
            <div class="card-body">
                <h5 class="card-title">
                    {{ character.name }}
                </h5>
                <p class="card-text">
                    Origin: {{ character.origin.name }}
                </p>
                <p class="card-text">
                    Last known location: {{ character.location.name }}
                </p>
            </div>
            <ul class="list-group list-group-flush">
                <li class="list-group-item">
                    Status: {{ character.status }}
                </li>
                <li class="list-group-item">
                    Gender: {{ character.gender }}
                </li>
                <li class="list-group-item">
                    Species: {{ character.species }}
                </li>
            </ul>
            <div class="card-body d-flex justify-content-end">
                <a href="#" class="btn btn-danger"
                @click.prevent='closeModal'>
                    Close
                </a>
            </div>
        </div>
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
        background-color: rgba(0, 0, 0, 0.6);
        display: flex;
        justify-content: center;
        align-items: center
    }
    button.btn-close{
        position: absolute;
        top: 10px;
        right: 10px;
    }

</style>