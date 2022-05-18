<template>
    <div class="search">
        <form @submit.prevent="SearchJokes()" class="search-box">
            <input v-model="search" type="text" placeholder="Need some Chuck jokes?">
        </form>
        <div class="joke-list">
            <div v-if="searchData.jokes">
                <div class="jokes" v-for="(joke, index) in searchData.jokes.slice(0, 10)" :key="joke.id">
                    <p @click="deleteJoke(index)" class="text">{{ joke.value }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { ref } from 'vue';

export default {
    setup() {
        const search = ref('')
        const searchData = ref([])
        const SearchJokes = () => {
            if(search.value != '') {
                axios.get(`https://api.chucknorris.io/jokes/search?query=${search.value}`)
                .then(response => {
                    searchData.value.jokes = response.data.result
                    search.value = ''
                })
            }
        }

        return {
            search,
            searchData,
            SearchJokes
        }
    },
    methods: {
        deleteJoke(index) {
            this.searchData.jokes.splice(index, 1);
        }
    }
}
</script>

<style lang="scss">
.search {
  
    .search-box {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        padding: 16px;
        input {
            display: block;
            appearance: none;
            border: none;
            outline: none;
            background: none;
            &[type="text"] {
                width: 100%;
                max-width: 300px;
                color: #FFF;
                background-color: #496583;
                font-size: 18px;
                padding: 10px 16px;
                border-radius: 8px;
                margin-bottom: 15px;
                transition: 0.4s;
                &::placeholder {
                color: #f3f3f3;
                }
                &:focus {
                box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
                }
            }
            &[type="submit"] {
                width: 100%;
                max-width: 150px;
                background-color: #42B883;
                padding: 16px;
                border-radius: 8px;
                color: #FFF;
                font-size: 16px;
                text-transform: uppercase;
                transition: 0.4s;
                &:active {
                background-color: #3B8070;
                }
            }
        }
    }
    .joke-list {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;

        .jokes {
            cursor: pointer;
            .text {
                background-color: #42B883;
                color: #FFF;
                border-radius: 8px;
                display: flex;
                flex-direction: column;
                justify-content: center;
                align-items: center;
                text-align: center;
                font-size: 16px;
                padding: 16px;
                max-width: 300px;
            }
        }
    }
}
</style>