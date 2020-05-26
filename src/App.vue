<template>
<div id="app" class="bg-gray-200 px-6 py-8">
    <img src="./assets/dog-logo.png" alt="logo" class=" h-16">
    <h1 class=" text-center text-5xl">Love dogs</h1>
    <div class=" flex flex-col sm:flex-row px-16 flex-grow m-4">
        <FilterSettings v-on:breedChange="onBreedChange" />
        <OneDogPage v-bind:chosenBreed="chosenBreed" />
    </div>
</div>
</template>

<script>
import OneDogPage from './components/OneDogPage'
import FilterSettings from './components/FilterSettings'

export default {
    name: 'App',
    data() {
        return {
            chosenBreed: null
        }
    },
    components: {
        OneDogPage,
        FilterSettings
    },
    methods: {
        onBreedChange: function (value) {
            this.chosenBreed = value;
        }
    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    color: #2c3e50;
}
</style>
