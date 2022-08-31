<template>
  <div id="app">

    <Jumbotron />

    <Pagination
    :nextPage='apiNextAddress'
    :prevPage='apiPrevAddress'
    @address='getApiList'
    />

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
    <Pagination
    :nextPage='apiNextAddress'
    :prevPage='apiPrevAddress'
    @address='getApiList'
    />
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
            apiPrevAddress : null,
            apiPageAddress : null,
            charactersList : null
        }
  },
  created: function(){
        this.getApiList(this.apiBaseAddress);
    },
  methods: {
      getApiList(apiBaseAddress){
          axios
          .get(apiBaseAddress)
          .then((result) => {
              this.charactersList = result.data.results;
              this.apiNextAddress = result.data.info.next;
              this.apiPrevAddress = result.data.info.prev;

              console.log(this.charactersList);
              console.log(this.apiNextAddress);
              console.log(this.apiPrevAddress);
          })
          .catch((error) => {
              console.error(error);
          })
      },
  }
}
</script>

<style lang="scss">
@import '@/style/style.scss';

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>
