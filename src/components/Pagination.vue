<template>
    <nav aria-label="">
        <ul class="pagination justify-content-center">
            <!-- first page LI only visible on more than 1 page-->
            <li class="page-item"
            v-if="(this.totalPages !== 1)"
            @click.prevent='setPage(1, null)'
            >
                <a class="page-link" href="#" tabindex="-1" aria-disabled="true">
                    First
                </a>
            </li>
            <!-- prev page LI -->
            <li class="page-item"
            :class="(this.prevPage == null) ? 'disabled' : '' "
            @click.prevent='nearbyPage(apiPrev, activePage, prev)'
            >
                <a class="page-link" href="#" tabindex="-1" aria-disabled="true">
                    Previous
                </a>
            </li>
            <!-- LIs only visible to keep 5 elements visible if there are enough pages -->
            <li class="page-item"
            v-if="(((this.totalPages - this.activePage) < 1) && ((this.activePage - 4) > 0))"
            @click.prevent='setPage((activePage - 4), null)'
            >
                <a class="page-link text-success" href="#">
                    {{ this.activePage - 4 }}
                </a>
            </li>
            <li class="page-item"
            v-if="(((this.totalPages - this.activePage) < 2) && ((this.activePage - 3) > 0))"
            @click.prevent='setPage((activePage - 3), null)'
            >
                <a class="page-link text-secondary" href="#">
                    {{ this.activePage - 3 }}
                </a>
            </li>
            <!-- previous two pages LIs -->
            <li class="page-item"
            v-if="(this.activePage > 2)"
            @click.prevent='setPage((activePage - 2), null)'
            >
                <a class="page-link text-danger" href="#">
                    {{ this.activePage - 2 }}
                </a>
            </li>
            <li class="page-item"
            v-if="(this.activePage > 1)"
            @click.prevent='setPage((activePage - 1), null)'
            >
                <a class="page-link text-warning" href="#">
                    {{ this.activePage - 1 }}
                </a>
            </li>
            <!-- active page LI -->
            <li class="page-item"
            :class="'active'"
            >
                <a class="page-link" href="#">
                    {{ this.activePage }}
                </a>
            </li>
            <!-- next two pages LIs -->
            <li class="page-item"
            v-if='((this.totalPages - this.activePage) > 0)'
            @click.prevent='setPage((activePage + 1), null)'
            >
                <a class="page-link text-warning" href="#">
                    {{ this.activePage + 1 }}
                </a>
            </li>
            <li class="page-item"
            v-if='((this.totalPages - this.activePage) > 1)'
            @click.prevent='setPage((activePage + 2), null)'
            >
                <a class="page-link text-danger" href="#">
                    {{ this.activePage + 2 }}
                </a>
            </li>
            <!-- LIs only visible to keep 5 elements visible if there are enough pages -->
            <li class="page-item"
            v-if="((this.activePage == 2) || (this.activePage == 1)) && (this.activePage !== this.totalPages) && ((this.activePage + 2) !== this.totalPages)"
            @click.prevent='setPage((activePage + 3), null)'
            >
                <a class="page-link text-secondary" href="#">
                    {{ this.activePage + 3 }}
                </a>
            </li>
            <li class="page-item"
            v-if="(this.activePage == 1) && (this.activePage !== this.totalPages) && ((this.activePage + 3) !== this.totalPages)"
            @click.prevent='setPage((activePage + 4), null)'
            >
                <a class="page-link text-success" href="#">
                    {{ this.activePage + 4 }}
                </a>
            </li>
            <!-- next page LI -->
            <li class="page-item"
            :class="(this.nextPage == null) ? 'disabled' : '' "
            @click.prevent='nearbyPage(apiNext, activePage, next)'>
                <a class="page-link" href="#">
                    Next
                </a>
            </li>
            <!-- last page LI only visible on more than 1 page-->
            <li class="page-item"
            v-if="(this.totalPages !== 1)"
            @click.prevent='setPage(totalPages, null)'
            >
                <a class="page-link" href="#" tabindex="-1" aria-disabled="true">
                    Last
                </a>
            </li>
        </ul>
    </nav>
</template>

<script>
export default {
    name: 'CardsPagination',
    props: ['nextPage', 'prevPage', 'pageCount', 'newActive', 'updatingAddress'],
    data: function(){
        return {
            //searchStart : true,
            apiNext : null,
            apiPrev : null,
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
        },
    },
    methods: {
        nearbyPage: function (address, active, direction){
            if(address !== null){
                this.setPage(active, direction)
                this.$emit('address', address)
            }
        },
        setPage(active, direction){
            if(direction == 'next'){
                this.activePage = active + 1;
            } else if (direction == 'prev') {
                this.activePage = active - 1;
            } else {
                this.activePage = active;
                this.$emit('userInput', {input : null , page : this.activePage});
            }
            this.$emit('active', this.activePage)
        }
    }
}
</script>

<style lang='scss' scoped>


</style>