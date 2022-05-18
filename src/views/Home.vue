<template>
    <div class="home">
        <button @click="getJoke()" class="get-more">Get more</button>
        <div class="joke-list">
            <p class="jokes">{{ startingJoke }}</p>
            <p v-if="error">{{ error }}</p>
            <div class="jokes" v-for="joke in jokes" :key="joke.id">
                <p>{{ joke }}</p>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { ref } from '@vue/reactivity'

export default {
    setup() {
        const startingJoke = ref('')
        const jokes = ref([])
        const error = ref('')

        return {
            startingJoke, 
            jokes,
            error
        }
    },
    methods: {
        getStartingJoke() {
            axios.get('https://api.chucknorris.io/jokes/random')
            .then((response) => {
                this.startingJoke = response.data.value
            })
            .catch((error) => {
                console.log(error)
                this.errorMsg = 'Error retrieving data'
            })
        },
        getJoke() {
            axios.get('https://api.chucknorris.io/jokes/random')
            .then((response) => {
                this.jokes.push(response.data.value)
            })
            .catch((error) => {
                console.log(error)
                this.error = 'Error retrieving data'
            })
        }
    },
    created() {
        this.getStartingJoke()
    }
}
</script>

<style lang="scss">
.home {

  .get-more {
    border: none;
    outline: none;
    background: none;
    width: 100%;
    max-width: 150px;
    background-color: #496583;
    padding: 16px;
    border-radius: 8px;
    color: #FFF;
    font-size: 16px;
    text-transform: uppercase;
    margin-bottom: 8px;
    transition: 0.4s;
    &:active {
      background-color: #3B8070;
    }
  }
  
  .joke-list {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    .jokes {
      margin: 8px;
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
      max-width: 400px;
    }
  }
}
</style>