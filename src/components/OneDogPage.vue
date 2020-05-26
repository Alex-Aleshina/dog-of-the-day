<template>
<div class="OneDogPage">
    <div class="flex flex-col justify-center items-center" v-if="chosenBreed.length > 0">
        <div>
            <button v-on:click='reloadImg' class=" focus:outline-none m-6 bg-green-700 hover:shadow-2xl hover:bg-green-500 active:bg-green-900 text-white font-bold py-2 px-4 border-green-600 shadow-lg rounded w-64 tracking-wider">Fetch new image</button>
        </div>

        <div class="px-8 py-4 max-w-4xl w-full">
            <img v-bind:src='info' alt='dog' class="imgOfDog rounded-md overflow-hidden shadow-2xl">
        </div>
    </div>
</div>
</template>

<script>
import axios from "axios"
export default {
    name: 'OneDogPage',
    props: {
        chosenBreed: {
            type: Array,
        }
    },
    data() {
        return {
            info: ""
        }
    },
    methods: {
        reloadImg: function () {
            axios
                .get(`https://dog.ceo/api/breed/${this.randomImgFromForm()}/images/random`)
                .then(response => (this.info = response.data.message));
        },
        randomImgFromForm: function () {
            let number = Math.floor(Math.random() * this.chosenBreed.length);
            return this.chosenBreed[number];
        }
    },
    watch: {
        chosenBreed() {
            this.reloadImg();
        }
    },
    mounted() {

    }
}
</script>
