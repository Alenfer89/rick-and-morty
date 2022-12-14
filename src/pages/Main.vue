<template>
  <main>
    <!-- Jumbotron with title and searchbar -->
    <Jumbotron @userInput="getFilteredChar" />
    <!-- Refresh and Favourites buttons -->
    <div class="d-flex justify-content-center p-4">
      <button class="btn btn-primary me-4" @click="resetAll()">
        Get All Characters!
      </button>
      <button class="btn btn-warning" @click="getFavourites(favList)">
        Your favs!
      </button>
    </div>
    <!-- top pagination component -->
    <Pagination
      v-if="showPagination"
      :nextPage="apiNextAddress"
      :prevPage="apiPrevAddress"
      :pageCount="apiPagesCount"
      :newActive="activePage"
      @address="getApiList"
      @active="changeActivePage"
      @userInput="getFilteredChar"
    />
    <!-- Char cards container plus modal cards -->
    <div class="container-fluid px-5">
      <ModalCard
        v-if="showModal"
        :character="modalChar"
        :arrayOfEpisodes="arrayOfEpisodes"
        @close="modalClose"
      />
      <div class="row px-5 justify-content-center">
        <div
          class="col-12 col-sm-10 col-md-6 col-lg-4 col-xxl-3 px-3 py-2"
          v-for="element in charactersList"
          :key="element.id"
        >
          <Card
            :name="element.name"
            :image="element.image"
            :status="element.status"
            :species="element.species"
            :charId="element.id"
            :favList="favList"
            @favId="changeFavStatus"
            @modalId="modalRequest"
          />
        </div>
        <div class="col-12" v-if="charactersList.length == 0">
          <h1 class="text-warning">
            {{ this.message }}
          </h1>
          <button
            class="btn btn-danger"
            v-show="!showLoader"
            @click="resetAll()"
          >
            Refresh!
          </button>
        </div>
      </div>
    </div>
    <!-- bottom pagination component -->
    <Pagination
      v-if="showPagination"
      :nextPage="apiNextAddress"
      :prevPage="apiPrevAddress"
      :pageCount="apiPagesCount"
      :newActive="activePage"
      @address="getApiList"
      @active="changeActivePage"
      @userInput="getFilteredChar"
    />
    <Loader v-if="showLoader" />
  </main>
</template>

<script>
//components
import Jumbotron from "../components/Jumbotron.vue";
import Card from "../components/Card.vue";
import ModalCard from "../components/ModalCard.vue";
import Pagination from "../components/Pagination.vue";
import Loader from "../components/Loader.vue";
//utilities
import axios from "axios";

export default {
  name: "MainPage",
  components: {
    Jumbotron,
    Card,
    ModalCard,
    Pagination,
    Loader,
  },
  data: function () {
    return {
      apiBaseAddress: "https://rickandmortyapi.com/api/character/",
      apiBaseEpisodeAddress: "https://rickandmortyapi.com/api/episode/",
      apiNextAddress: "",
      apiPrevAddress: "",
      apiPagesCount: null,
      activePage: 1,
      charactersList: [],
      stringToSearch: "",
      favList: [],
      showPagination: false,
      message: "Loading..",
      modalChar: {},
      showModal: false,
      showLoader: true,
      arrayOfEpisodes: [],
    };
  },
  created: function () {
    setTimeout(() => {
      this.getApiList(this.apiBaseAddress);
    }, 2000);
    //this.getApiList(this.apiBaseAddress);
  },
  methods: {
    getApiList(address) {
      this.showPagination = true;
      this.showLoader = false;
      axios
        .get(address)
        .then((result) => {
          this.charactersList = result.data.results;
          this.apiNextAddress = result.data.info.next;
          this.apiPrevAddress = result.data.info.prev;
          this.apiPagesCount = result.data.info.pages;
          //this.showLoader = false;
        })
        .catch((error) => {
          console.error(error);
          console.log("ricerca errata");
          this.charactersList = [];
          this.message = `Your search of "${this.stringToSearch}" didn't produce any result`;
        });
    },
    changeActivePage(number) {
      this.activePage = number;
    },
    getFilteredChar({ input, page }) {
      if (page == null) {
        this.stringToSearch = input;
        page = 1;
        this.activePage = page;
      } else if (input == null) {
        input = this.stringToSearch;
      }
      let newAddress = this.apiBaseAddress + "?page=" + page + "&name=" + input;
      this.getApiList(newAddress);
    },
    getFavourites(array) {
      this.showPagination = false;
      let newAddress = this.apiBaseAddress + array;
      axios
        .get(newAddress)
        .then((result) => {
          if (this.favList.length > 1) {
            this.charactersList = result.data;
          } else if (this.favList.length == 1) {
            let charObject = result.data;
            this.charactersList = [];
            this.charactersList.push(charObject);
          } else {
            this.charactersList = [];
            this.message = "You don't have any favourite character yet!";
          }
        })
        .catch((error) => {
          console.error(error);
          console.log("ricerca errata");
          this.charactersList = [];
        });
    },
    resetAll() {
      this.charactersList = [];
      this.apiNextAddress = "";
      this.apiPrevAddress = "";
      this.apiPagesCount = null;
      this.getApiList(this.apiBaseAddress), (this.activePage = 1);
      this.stringToSearch = "";
    },
    changeFavStatus(id) {
      if (!this.favList.includes(id)) {
        this.favList.push(id);
      } else {
        const charIndex = this.favList.indexOf(id);
        if (charIndex > -1) {
          this.favList.splice(charIndex, 1);
        }
      }
    },
    modalRequest(id) {
        const character = this.charactersList.find(element => element.id == id)
        const episodesIds = character.episode.map(ep => ep.slice(40))
          let newAddress = this.apiBaseEpisodeAddress + episodesIds;
          console.log(newAddress)
          axios
            .get(newAddress)
            .then((result) => {
                if(Array.isArray(result.data)){
                    this.arrayOfEpisodes = result.data;
                } else {
                    this.arrayOfEpisodes = [result.data];
                }
                this.arrayOfEpisodes = this.arrayOfEpisodes.map(ep => ep.episode + ' - ' + ep.name)
              this.modalChar = character;
                this.showModal = true;
                document.body.classList.add("modal-on");
            })
            .catch((error) => {
              console.error(error);
            });
    },
    modalClose(boolean) {
      this.modalChar = {};
      this.showModal = boolean;
      document.body.classList.remove("modal-on");
    },
  },
};
</script>

<style lang="scss">
@import "@/style/style.scss";

.modal-on {
  overflow: hidden;
}
</style>
