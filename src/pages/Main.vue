<template>
    <main>
<!-- Jumbotron with title and searchbar -->
        <Jumbotron
        @userInput='getFilteredChar'
        />
<!-- Refresh and Favourites buttons -->
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
<!-- top pagination component -->
        <Pagination
        v-if='!showFavs'
        :nextPage='apiNextAddress'
        :prevPage='apiPrevAddress'
        :pageCount='apiPagesCount'
        :newActive='activePage'
        @address='getApiList'
        @active='changeActivePage'
        @userInput='getFilteredChar'
        />
<!-- Char cards container plus modal cards -->
        <div class="container-fluid px-5">
            <ModalCard
            v-if="showModal"
            :character='modalChar'
            @close='modalClose'
            />
            <div class="row px-5 justify-content-center">
                <div class="col-12 col-sm-10 col-md-6 col-lg-4 col-xxl-3 px-3 py-2"
                v-for="element in charactersList" 
                :key="element.id">

                    <Card
                    :name="element.name"
                    :image="element.image"
                    :status="element.status"
                    :species="element.species"
                    :charId="element.id"
                    :favList='favList'
                    @favId='changeFavStatus'
                    @modalId='modalRequest'
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
        </div>
<!-- bottom pagination component -->
        <Pagination
        v-if='!showFavs'
        :nextPage='apiNextAddress'
        :prevPage='apiPrevAddress'
        :pageCount='apiPagesCount'
        :newActive='activePage'
        @address='getApiList'
        @active='changeActivePage'
        @userInput='getFilteredChar'
        />
    </main>
</template>

<script>
//components
import Jumbotron from '../components/Jumbotron.vue';
import Card from '../components/Card.vue';
import ModalCard from '../components/ModalCard.vue';
import Pagination from '../components/Pagination.vue';
//utilities
import axios from "axios";

export default {
    name: 'MainPage',
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
            showFavs : false,
            message : '',
            modalChar : {},
            showModal : false
        }
    },
    created: function(){
            this.getApiList(this.apiBaseAddress);
    },
    methods: {
        getApiList(address){
            this.showFavs = false;
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
            this.showFavs = true;
            let newAddress = this.apiBaseAddress + array;
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
            this.charactersList = [];
            this.apiNextAddress = '';
            this.apiPrevAddress = '';
            this.apiPagesCount = null;
            this.getApiList(this.apiBaseAddress),
            this.activePage = 1;
            this.stringToSearch = '';
        },
        changeFavStatus(id){
            if(!this.favList.includes(id)){
                this.favList.push(id);
            } else {
                const charIndex = this.favList.indexOf(id);
                if (charIndex > -1) { 
                    this.favList.splice(charIndex, 1);
                }
            }
        },
        modalRequest(id){
            console.warn(id)
            this.charactersList.forEach(element => {
                if(element.id == id){
                    this.modalChar = element;
                    this.showModal = true;
                    document.body.classList.add("modal-on");
                }
            })
        },
        modalClose(boolean){
            this.modalChar = {};
            this.showModal = boolean;
            document.body.classList.remove("modal-on");
        }
    }
}


</script>

<style lang="scss">
    @import '@/style/style.scss';

    .modal-on{
        overflow: hidden;
    }
</style>