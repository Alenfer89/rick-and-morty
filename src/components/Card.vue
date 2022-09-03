<template>
    <div class="card shadow">
        <img :src="image" class="p-3 card-img-top rounded-circle mb-2" :alt="'image of ' + name">
        <div class="card-body text-center">
            <h5 class="card-title">
                Name: {{ name }}
            </h5>
            <p class="card-text m-2 fw-bold">
                Status: {{ status }}
            </p>
            <p class="card-text">
                Ethnicity: {{ species }}
            </p>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SingleCard',
    props: ['name', 'image', 'status', 'species'],
}
</script>

<style>

</style>