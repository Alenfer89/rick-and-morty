<template>
  <div id="app">
    <header class="container-fluid">
      <Jumbotron />
    </header>
    <div class="container-fluid p-5">
      <div class="row p-5">
        <div class="col-3 px-4 py-3"
        v-for="element in charactersList" 
        :key="element.id">
          <Card
          :title="element.name"
          :image="element.image"
          :description="element.status"
          :boldDescription="element.species"
          />
        </div>
      </div>
      
      <ModalCard />
      <Favourite />
    </div>
    <Pagination />
  </div>
</template>

<script>
//components
import Jumbotron from './components/Jumbotron.vue';
import Card from './components/Card.vue';
import ModalCard from './components/ModalCard.vue';
import Pagination from './components/Pagination.vue';
import Favourite from './components/Favourite.vue';
//utilities
import axios from "axios";

export default {
  name: 'App',
  components: {
    Jumbotron,
    Card,
    ModalCard,
    Pagination,
    Favourite,
    //Loader
  },
  data: function(){
        return{
            apiBaseAddress: "https://rickandmortyapi.com/api/character",
            apiNextAddress : null,
            charactersList : null,
        }
  },
  created: function(){
        this.getApiList();
    },
    methods: {
        getApiList(){
            //console.log(apiAddress);
            axios
            .get('https://rickandmortyapi.com/api/character')
            .then((result) => {
                // console.warn(result.data.info.next);
                this.charactersList = result.data.results;
                this.apiNextAddress = result.data.info.next;

                console.log(this.charactersList);
            })
            .catch((error) => {
                console.error(error);
            })
        },
    },
}
</script>

<style lang="scss">
@import '@/style/style.scss';

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>
