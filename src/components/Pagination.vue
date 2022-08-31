<template>
    <nav aria-label="">
        <ul class="pagination justify-content-center">
            <li class="page-item"
            :class="(this.prevPage == null) ? 'disabled' : '' "
            @click.prevent='changePage(apiPrev)'>
                <a class="page-link" href="" tabindex="-1" aria-disabled="true">
                    Previous
                </a>
            </li>
            <li class="page-item">
                <a class="page-link" href="#">
                    1
                </a>
            </li>
            <li class="page-item">
                <a class="page-link" href="#">
                    2
                </a>
            </li>
            <li class="page-item">
                <a class="page-link" href="#">
                    3
                </a>
            </li>
            <li class="page-item"
            :class="(this.nextPage == null) ? 'disabled' : '' "
            @click.prevent='changePage(apiNext)'>
                <a class="page-link" href="">
                    Next
                </a>
            </li>
        </ul>
    </nav>
</template>

<script>
export default {
    name: 'CardsPagination',
    props: ['nextPage', 'prevPage'],
    data: function(){
        return {
            searchStart : true,
            apiNext : this.nextPage,
            apiPrev : this.prevPage,
            activePage : 1,
        }
    },
    watch:{
        nextPage(){
            this.apiNext = this.nextPage;
        },
        prevPage(){
            this.apiPrev = this.prevPage;
        },
    },
    methods: {
        changePage(address){
            if(address !== null){
                this.$emit('address', address)
            }
        }
    }
}
</script>

<style lang='scss' scoped>


</style>