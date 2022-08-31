<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <Jumbotron />
    <Card />
    <ModalCard />
    <Pagination />
    <Favourite />
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
    Favourite
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
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
