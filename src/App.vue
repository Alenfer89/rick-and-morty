<template>
  <div id="app">

    <Jumbotron
    @userInput='getFilteredChar'
    />
    <div class="d-flex justify-content-center p-4">
      <button class="btn btn-primary me-4"
      @click='resetAll()'>
        Get All Characters!
      </button>
      <button class="btn btn-warning"
      @click='getFavourites(favList)'>
        Your favs!
      </button>
    </div>

    <Pagination
    :nextPage='apiNextAddress'
    :prevPage='apiPrevAddress'
    :pageCount='apiPagesCount'
    :newActive='activePage'
    @address='getApiList'
    @active='changeActivePage'
    @userInput='getFilteredChar'
    />

    <div class="container-fluid px-5">
      <div class="row px-5">
        <div class="col-3 px-4 py-3"
        v-for="element in charactersList" 
        :key="element.id">

          <Card
          :name="element.name"
          :image="element.image"
          :status="element.status"
          :species="element.species"
          :charId="element.id"
          :favList='favList'
          @id='changeFavStatus'
          />

        </div>
        <div class="col-12"
        v-if='(charactersList.length == 0)'
        >
          <h2 class="text-warning">
            {{ this.message }}
          </h2>
          <button class="btn btn-danger"
          @click='resetAll()'>
            Refresh!
          </button>
        </div>
      </div>
      
      <ModalCard />
    </div>
    <Pagination
    :nextPage='apiNextAddress'
    :prevPage='apiPrevAddress'
    :pageCount='apiPagesCount'
    :newActive='activePage'
    @address='getApiList'
    @active='changeActivePage'
    @userInput='getFilteredChar'
    />
  </div>
</template>

<script>
//components
import Jumbotron from './components/Jumbotron.vue';
import Card from './components/Card.vue';
import ModalCard from './components/ModalCard.vue';
import Pagination from './components/Pagination.vue';
//utilities
import axios from "axios";
//import bootstrap from "~bootstrap/scss/bootstrap.scss";

export default {
  name: 'App',
  components: {
    Jumbotron,
    Card,
    ModalCard,
    Pagination,
    //Loader
  },
  data: function(){
        return{
            apiBaseAddress: "https://rickandmortyapi.com/api/character/",
            apiNextAddress : '',
            apiPrevAddress : '',
            apiPagesCount : null,
            activePage : 1,
            charactersList : [],
            stringToSearch : '',
            favList : [],
            message : '',
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
                this.charactersList = result.data.results;
                this.apiNextAddress = result.data.info.next;
                this.apiPrevAddress = result.data.info.prev;
                this.apiPagesCount = result.data.info.pages;
            })
            .catch((error) => {
                console.error(error);
                console.log('ricerca errata')
                this.charactersList = [];
                this.message = `Your search of "${this.stringToSearch}" didn't produce any result`;
            })
        },
        changeActivePage(number){
            this.activePage = number;
        },
        getFilteredChar({input, page}){
            if (page == null){
              this.stringToSearch = input;
              page = 1;
              this.activePage = page;
            } else if (input == null) {
              input = this.stringToSearch;
            }
            let newAddress = this.apiBaseAddress + '?page=' + page + '&name=' + input;
            this.getApiList(newAddress);
        },
        getFavourites(array){
            let newAddress = this.apiBaseAddress + array;
            console.log(newAddress)
            axios
            .get(newAddress)
            .then((result) => {
                if(this.favList.length > 1){
                    this.charactersList = result.data
                } else if(this.favList.length == 1){
                    let charObject = result.data;
                    this.charactersList = [];
                    this.charactersList.push(charObject)
                } else {
                    this.charactersList = [];
                    this.message = 'You don\'t have any favourite character yet!'
                }
            })
            .catch((error) => {
                console.error(error);
                console.log('ricerca errata')
                this.charactersList = [];
            })
        },
        resetAll(){
            this.getApiList(this.apiBaseAddress),
            this.activePage = 1;
            this.stringToSearch = '';
        },
        changeFavStatus(id){
            console.error(id)
            if(!this.favList.includes(id)){
              this.favList.push(id);
              console.log('aggiunto')
              console.log(this.favList)
            } else {
              const charIndex = this.favList.indexOf(id);
              if (charIndex > -1) { 
                this.favList.splice(charIndex, 1); 
                console.log('extra-check')
              }
              console.log('rimosso')
              console.warn(this.favList)
            }
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
