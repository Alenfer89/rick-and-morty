<template>
    <div class="card shadow position-relative animate__animated animate__fadeInLeft">
        <span class="ax-favourite"
        @click='changeFavStatus(charId)'>
            <i class="fa-solid fa-star" v-if='isLiked'></i>
            <i class="fa-regular fa-star" v-else></i>
        </span>
        <span class="ax-popup rounded-pill" :class="isLiked ? 'text-danger' : 'text-warning'">
            {{ isLiked ? 'Remove from your favourites' : 'Add to favourites!' }}
        </span>
        <img :src="image" class="p-3 mt-3 card-img-top rounded-circle" :alt="'image of ' + name">
        <div class="card-body d-flex flex-column">
            <span>
                Name:
            </span>
            <h5 class="card-title fw-bold">
                {{ name.length > 20 ? name.substring(0, 18) + '..' : name }}
            </h5>
            <span>
                Status:
            </span>
            <p class="card-text fw-bold"
            :class="(status == 'Alive') ? 'text-success' : (status == 'Dead') ? 'text-danger' : (status == 'unknown') ? 'text-warning' : ''">
                {{ status }}
            </p>
            <span>
                Species:
            </span>
            <p class="card-text fw-bold">
                {{ species }}
            </p>
        </div>
        <div class="card-footer d-flex justify-content-between">
            <a href="#" class="btn btn-primary btn-sm"
            @click.prevent="showModal(charId)">
                More info..
            </a>
            <a href="#" class="btn btn-secondary disabled btn-sm">
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