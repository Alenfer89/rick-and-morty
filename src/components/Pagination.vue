<template>
    <nav aria-label="">
        <ul class="pagination justify-content-center py-3 mb-0">
            <!-- first page LI only visible on more than 1 page-->
            <li class="page-item"
            :class="(this.activePage == 1) ? 'disabled' : '' "
            v-if="(this.totalPages !== 1)"
            @click.prevent='setPage(1, null)'
            >
                <a class="page-link shadow-none" href="#">
                    <i class="fa-solid fa-angles-left"></i>
                    <span class="d-none d-sm-inline"> 
                        First
                    </span>
                </a>
            </li>
            <!-- prev page LI -->
            <li class="page-item"
            :class="(this.prevPage == null) ? 'disabled' : '' "
            @click.prevent='nearbyPage(apiPrev, activePage, prev)'
            >
                <a class="page-link shadow-none" href="#" tabindex="-1" aria-disabled="true">
                    <i class="fa-solid fa-angle-left"></i>
                    <span class="d-none d-sm-inline"> 
                        Previous
                    </span>
                </a>
            </li>
            <!-- LIs only visible to keep 5 elements visible if there are enough pages -->
            <li class="page-item"
            v-if="(((this.totalPages - this.activePage) < 1) && ((this.activePage - 4) > 0))"
            @click.prevent='setPage((activePage - 4), null)'
            >
                <a class="page-link shadow-none text-success" href="#">
                    {{ this.activePage - 4 }}
                </a>
            </li>
            <li class="page-item"
            v-if="(((this.totalPages - this.activePage) < 2) && ((this.activePage - 3) > 0))"
            @click.prevent='setPage((activePage - 3), null)'
            >
                <a class="page-link shadow-none text-secondary" href="#">
                    {{ this.activePage - 3 }}
                </a>
            </li>
            <!-- previous two pages LIs -->
            <li class="page-item"
            v-if="(this.activePage > 2)"
            @click.prevent='setPage((activePage - 2), null)'
            >
                <a class="page-link shadow-none text-danger" href="#">
                    {{ this.activePage - 2 }}
                </a>
            </li>
            <li class="page-item"
            v-if="(this.activePage > 1)"
            @click.prevent='setPage((activePage - 1), null)'
            >
                <a class="page-link shadow-none text-warning" href="#">
                    {{ this.activePage - 1 }}
                </a>
            </li>
            <!-- active page LI -->
            <li class="page-item"
            :class="'active'"
            >
                <a class="page-link shadow-none" href="#">
                    {{ this.activePage }}
                </a>
            </li>
            <!-- next two pages LIs -->
            <li class="page-item"
            v-if='((this.totalPages - this.activePage) > 0)'
            @click.prevent='setPage((activePage + 1), null)'
            >
                <a class="page-link shadow-none text-warning" href="#">
                    {{ this.activePage + 1 }}
                </a>
            </li>
            <li class="page-item"
            v-if='((this.totalPages - this.activePage) > 1)'
            @click.prevent='setPage((activePage + 2), null)'
            >
                <a class="page-link shadow-none text-danger" href="#">
                    {{ this.activePage + 2 }}
                </a>
            </li>
            <!-- LIs only visible to keep 5 elements visible if there are enough pages -->
            <li class="page-item"
            v-if="((this.activePage == 2) || (this.activePage == 1)) && (this.activePage !== this.totalPages) && ((this.activePage + 2) !== this.totalPages)"
            @click.prevent='setPage((activePage + 3), null)'
            >
                <a class="page-link shadow-none text-secondary" href="#">
                    {{ this.activePage + 3 }}
                </a>
            </li>
            <li class="page-item"
            v-if="(this.activePage == 1) && (this.activePage !== this.totalPages) && ((this.activePage + 3) !== this.totalPages)"
            @click.prevent='setPage((activePage + 4), null)'
            >
                <a class="page-link shadow-none text-success" href="#">
                    {{ this.activePage + 4 }}
                </a>
            </li>
            <!-- next page LI -->
            <li class="page-item"
            :class="(this.nextPage == null) ? 'disabled' : '' "
            @click.prevent='nearbyPage(apiNext, activePage, next)'>
                <a class="page-link shadow-none" href="#">
                    <span class="d-none d-sm-inline"> 
                        Next
                    </span>
                    <i class="fa-solid fa-angle-right"></i>
                </a>
            </li>
            <!-- last page LI only visible on more than 1 page-->
            <li class="page-item"
            :class="(this.activePage == this.totalPages) ? 'disabled' : '' "
            v-if="(this.totalPages !== 1)"
            @click.prevent='setPage(totalPages, null)'
            >
                <a class="page-link shadow-none"  href="#">
                    <span class="d-none d-sm-inline"> 
                        Last
                    </span>
                    <i class="fa-solid fa-angles-right"></i>
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
.page-link.shadow-none{
    cursor: pointer;
}

li{
    box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15);
    &:first-child{
        border-radius: 0.375rem 0 0 0.375rem;
    }
    &:last-child{
        border-radius: 0 0.375rem 0.375rem 0;
    }
}

</style>