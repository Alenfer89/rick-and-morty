<template>

    <header class="ax-jumbo-bg position-relative">
        <div class="ax-layer">
            <!-- reading helper d/t image -->
        </div>
        <div class="container-fluid ax-above-layer">
            <div class="row">
                <div class="col-12 d-flex justify-content-center align-items-center">
                    <img class="" src="https://upload.wikimedia.org/wikipedia/commons/b/b1/Rick_and_Morty.svg" alt="Title of Rick and Morty tv series">
                </div>
                <div class="col-12 d-flex justify-content-center align-items-center">
                    <div class="input-group w-25">
                        <div class="input-group-prepend">
                            <span class="input-group-text" id="basic-addon1">
                                @
                            </span>
                        </div>
                        <input type="text" class="form-control" placeholder="Username" aria-label="Username" aria-describedby="basic-addon1">
                    </div>
                </div>
            </div>
        </div>
    </header>

</template>

<script>
export default {
    name: 'JumbotronAndSearch'
}
</script>

<style lang='scss' scoped>

    header.ax-jumbo-bg{
        padding: 8rem 0;
        background-image: url('https://www.pointblank.it/sites/default/files/styles/slide/public/2016-09/Rick_e_Morty_recensione_serie_tv.jpg?itok=YuGPLNpb');
        background-position: center;
        background-repeat: no-repeat;
        background-size: cover;
        width: 100%;
        //height: 50vh;
        z-index: 0;

        div.ax-layer{
            position: absolute;
            z-index: 1;
            top: 0;
            right: 0;
            bottom: 0;
            left: 0;
            background-color: rgba(0, 0, 0, 0.4);
        }
        div.ax-above-layer{
            position: relative;
            z-index: 2;
            img{
                width: 60%;
                min-width: 250px;
            }
        }
    }

</style>