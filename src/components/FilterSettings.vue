<template>
<div class='FilterSettings'>
    <div class="flex justify-center items-center">
        <select v-model="selected" multiple v-on:change="updateValue" class="breedsList block w-64 sm:w-10/12 bg-white border-2 border-gray-400 hover:border-gray-500 px-5 py-2 pr-10 rounded shadow focus:outline-none focus:shadow-outline tracking-wider h-64 text-sm">
            <option v-for="(name, id) in breeds" v-bind:value="id" v-bind:key="id">
                {{ name  | uppercase}}
            </option>
        </select>
    </div>
    <div class="chosenOptions m-6 flex flex-wrap justify-around">
        <label for="includeBreed">
            <div class="flex items-center text-center h-24 w-48 sm:w-64 m-4 border-2 rounded-md p-5 hover:shadow-2xl hover:bg-green-100 active:bg-green-300">
                <input type="radio" id="includeBreed" value="includeBreed" v-model="picked" v-on:change="updateValue">
                <p class="p-4">I want the following breeds</p>
            </div>
        </label>
        <label for="excludeBreed">
            <div class="flex items-center text-center h-24 w-48 sm:w-64 m-4 border-2 rounded-md p-5 hover:shadow-2xl hover:bg-green-100 active:bg-green-300">
                <input type="radio" id="excludeBreed" value="excludeBreed" v-model="picked" v-on:change="updateValue">
                <p>I want to exclude the following breeds</p>
            </div>
        </label>
    </div>

</div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'FilterSettings',
    data() {
        return {
            breeds: {

            },
            selected: [],
            picked: 'includeBreed',
            errored: false,
            loading: true,
        }
    },
    methods: {
        updateValue: function () {
            this.saveInLocalStorage();
            if (this.picked === "includeBreed") {
                this.$emit('breedChange', this.selected);
            } else {
                this.$emit('breedChange', this.excludeBreed());
            }
        },
        excludeBreed: function () {
            let allBreeds = Object.keys(this.breeds);

            let result = [];

            allBreeds.forEach((breed) => {
                if (this.selected.indexOf(breed) === -1) {
                    result.push(breed);
                }
            })
            return result;

        },
        saveInLocalStorage: function () {
            const selectedJson = JSON.stringify(this.selected);
            localStorage.selected = selectedJson;
            localStorage.picked = this.picked;
        }

    },
    filters: {
        uppercase: function (value) {
            return value.toUpperCase();
        }
    },
    mounted() {
        axios
            .get('https://dog.ceo/api/breeds/list/all')
            .then(response => {
                let result = {};
                let breeds = response.data.message;
                for (let breed in breeds) {
                    if (breeds[breed].length > 0) {
                        for (let i = 0; i < breeds[breed].length; i++) {
                            let name = breeds[breed][i] + ' ' + breed;
                            let id = breed + '/' + breeds[breed][i];
                            result[id] = name;
                        }
                    } else {
                        let name = breed;
                        let id = breed;
                        result[id] = name;
                    }
                }
                this.breeds = result;
                this.updateValue();
            })
            .catch(error => {
                console.log(error)
                this.errored = true;
            })
            .finally(() => (this.loading = false));

        if (localStorage.selected) {
            this.selected = JSON.parse(localStorage.selected);
        }
        if (localStorage.picked) {
            this.picked = localStorage.picked;
        }
    }
}
</script>
