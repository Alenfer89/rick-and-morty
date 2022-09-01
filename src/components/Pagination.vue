<template>
    <nav aria-label="">
        <ul class="pagination justify-content-center">
            <li class="page-item"
            :class="(this.prevPage == null) ? 'disabled' : '' "
            @click.prevent='changePage(apiPrev, activePage, prev)'
            >
                <a class="page-link" href="#" tabindex="-1" aria-disabled="true">
                    Previous
                </a>
            </li>
            <li class="page-item" 
            v-if="((this.totalPages - this.activePage) < 2)"
            @click.prevent='changePage(apiByPage + (activePage - 2), (activePage - 2), null)'
            >
                <a class="page-link" href="#">
                    {{ this.activePage - 4 }}
                </a>
            </li>
            <li class="page-item" 
            v-if="((this.totalPages - this.activePage) < 1)"
            @click.prevent='changePage(apiByPage + (activePage - 2), (activePage - 2), null)'
            >
                <a class="page-link" href="#">
                    {{ this.activePage - 3 }}
                </a>
            </li>
            <li class="page-item" 
            v-if="(this.activePage > 2)"
            @click.prevent='changePage(apiByPage + (activePage - 2), (activePage - 2), null)'
            >
                <a class="page-link" href="#">
                    {{ this.activePage - 2 }}
                </a>
            </li>
            <li class="page-item" 
            v-if="(this.activePage > 1)"
            @click.prevent='changePage(apiByPage + (activePage - 1), (activePage - 1), null)'
            >
                <a class="page-link" href="#">
                    {{ this.activePage - 1 }}
                </a>
            </li>
            <li class="page-item" 
            :class="'active'"
            >
                <a class="page-link" href="#">
                    {{ this.activePage }}
                </a>
            </li>
            <li class="page-item"
            v-if='((this.totalPages - this.activePage) > 0)'
            @click.prevent='changePage(apiByPage + (activePage + 1), (activePage + 1), null)'
            >
                <a class="page-link" href="#">
                    {{ this.activePage + 1 }}
                </a>
            </li>
            <li class="page-item"
            v-if='((this.totalPages - this.activePage) > 1)'
            @click.prevent='changePage(apiByPage + (activePage + 2), (activePage + 2), null)'
            >
                <a class="page-link" href="#">
                    {{ this.activePage + 2 }}
                </a>
            </li>
            <li class="page-item" 
            v-if="(this.activePage == 2) || (this.activePage == 1)"
            @click.prevent='changePage(apiByPage + (activePage + 3), (activePage + 3), null)'
            >
                <a class="page-link" href="#">
                    {{ this.activePage + 3 }}
                </a>
            </li>
            <li class="page-item" 
            v-if="(this.activePage == 1)"
            @click.prevent='changePage(apiByPage + (activePage + 4), (activePage + 4), null)'
            >
                <a class="page-link" href="#">
                    {{ this.activePage + 4 }}
                </a>
            </li>
            <li class="page-item"
            :class="(this.nextPage == null) ? 'disabled' : '' "
            @click.prevent='changePage(apiNext, activePage, next)'>
                <a class="page-link" href="#">
                    Next
                </a>
            </li>
        </ul>
    </nav>
</template>

<script>
export default {
    name: 'CardsPagination',
    props: ['nextPage', 'prevPage', 'pageCount', 'newActive'],
    data: function(){
        return {
            //searchStart : true,
            apiNext : null,
            apiPrev : null,
            apiByPage : "https://rickandmortyapi.com/api/character?page=",
            activePage : 1,
            totalPages : null,
            next : 'next',
            prev : 'prev',
        }
    },
    watch:{
        nextPage(){
            this.apiNext = this.nextPage;
        },
        prevPage(){
            this.apiPrev = this.prevPage;
        },
        pageCount(){
            this.totalPages = this.pageCount;
        },
        newActive(){
            this.activePage = this.newActive;
        }
    },
    methods: {
        changePage(address, active, direction){
            if(address !== null){
                console.error(address)
                console.error(active)
                console.error(direction)
                this.changeActive(active, direction)
                this.$emit('address', address)
                //this.$emit('active', active)
            }
        },
        changeActive(active, direction){
            if(direction == 'next'){
                this.activePage = active + 1;
            } else if (direction == 'prev') {
                this.activePage = active - 1;
            } else {
                this.activePage = active;
            }
            this.$emit('active', this.activePage)
        }
    }
}
</script>

<style lang='scss' scoped>
.active{
    background-color: aqua;
}

</style>