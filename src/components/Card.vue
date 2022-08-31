<template>
    <div class="card shadow">
        <img :src="image" class="p-3 card-img-top rounded-circle mb-2" alt="...">
        <div class="card-body text-center">
            <h5 class="card-title">{{title}}</h5>
            <p class="card-text m-2">{{description}}</p>
            <p class="card-text fw-bold">{{boldDescription}}</p>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SingleCard',
    props: ['title', 'image', 'description', 'boldDescription'],
}
</script>

<style>

</style>