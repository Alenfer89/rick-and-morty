<template>
  <div id="app">

    <Jumbotron 
    :updatingAddress='apiAddress'
    @address='getApiList'
    />

    <Pagination
    :nextPage='apiNextAddress'
    :prevPage='apiPrevAddress'
    :pageCount='apiPagesCount'
    :newActive='activePage'
    :updatingAddress='apiAddress'
    @address='getApiList'
    @active='changeActivePage'
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
    :pageCount='apiPagesCount'
    :newActive='activePage'
    :updatingAddress='apiAddress'
    @address='getApiList'
    @active='changeActivePage'
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
            apiAddress : null,
            apiNextAddress : null,
            apiPrevAddress : null,
            apiPagesCount : null,
            activePage : 1,
            charactersList : null
        }
  },
  created: function(){
        this.getApiList(this.apiBaseAddress);
  },
  methods: {
      getApiList(address){
          axios
          .get(address)
          .then((result) => {
              this.apiAddress = address;
              this.charactersList = result.data.results;
              this.apiNextAddress = result.data.info.next;
              this.apiPrevAddress = result.data.info.prev;
              this.apiPagesCount = result.data.info.pages;

              console.log(this.charactersList);
              console.log(this.apiNextAddress);
              console.log(this.apiPrevAddress);
              console.warn(this.apiPagesCount);
          })
          .catch((error) => {
              console.error(error);
          })
      },
      changeActivePage(number){
        this.activePage = number;
        return this.activePage;
      },
      addressComposer(){
        
      }
  }
}
</script>

<style lang="scss">
@import '@/style/style.scss';

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>
