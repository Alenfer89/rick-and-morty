<template>
    <div class="card shadow position-relative">
        <span class="ax-favourite"
        @click='changeFavStatus(charId)'>
            <i class="fa-solid fa-star" v-if='isLiked'></i>
            <i class="fa-regular fa-star" v-else></i>
        </span>
        <span class="ax-popup rounded-pill" :class="isLiked ? 'text-danger' : 'text-warning'">
            {{ isLiked ? 'Remove from your favourites' : 'Add to favourites!' }}
        </span>
        <img :src="image" class="p-3 card-img-top rounded-circle mb-2" :alt="'image of ' + name">
        <div class="card-body text-center">
            <h5 class="card-title">
                Name: {{ name }}
            </h5>
            <p class="card-text m-2 fw-bold">
                Status: {{ status }}
            </p>
            <p class="card-text">
                Ethnicity: {{ species }}
            </p>
        </div>
        <div class="card-footer d-flex justify-content-between">
            <a href="#" class="card-link"
            @click.prevent="showModal(charId)">
                More info..
            </a>
            <a href="#" class="card-link">
                Episodes
            </a>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SingleCard',
    props: ['name', 'image', 'status', 'species', 'charId', "favList"],
    data: function(){
        return{
            isLiked : false
        }
    },
    mounted(){
        if(this.favList.includes(this.charId)){
            this.isLiked = true;
        }
        
    },
    methods: {
        changeFavStatus(id){
            this.isLiked = !this.isLiked;
            this.$emit('favId', id);
        },
        showModal(id){
            this.$emit('modalId', id);
        }
    }
}
</script>

<style lang='scss' scoped>

    .ax-popup{
        display: none;
        position: absolute;
        top: -20px;
        right: -25px;
        padding: .5rem;
        //color: rgb(255, 200, 0);
        border: .5px solid goldenrod;
        background-color: rgb(250, 250, 250);
        font-size: .7rem;
    }
    .ax-favourite{
        position: absolute;
        top: 0;
        right: 0;
        padding: .5rem;
        color: rgb(255, 200, 0);
        font-size: 2rem;
    }
    .ax-favourite:hover + .ax-popup{
            display: inline-block;
        }

</style>