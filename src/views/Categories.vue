<template>
    <div class="flex-container">
        <p v-if="error">{{ error }}</p>
        <div class="flex-child">
            <h3>Category List</h3>
            <div v-for="catName in categories" :key="catName.id" class="categories">
                <p class="cat" @click="getJoke(catName)">{{ catName }}</p>
            </div>
        </div>
        <div class="flex-child">
            <h3>Joke List</h3>
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
        const categories = ref([])
        const jokes = ref([])
        const error = ref('')

        return {
            categories,
            jokes,
            error
        }
    },
    methods: {
        getCategories() {
            axios.get('https://api.chucknorris.io/jokes/categories')
            .then((response) => {
                this.categories = response.data
            })
            .catch((error) => {
                console.log(error)
                this.error = 'Error retrieving data'
            })
        },
        getJoke(catName) {
            axios.get('https://api.chucknorris.io/jokes/random', {
                params: { category: catName }
            })
            .then((response) => {
                this.jokes.unshift(response.data.value)
            })
            .catch((error) => {
                console.log(error)
                this.errorMsg = 'Error retrieving data'
            })
        }
    },
    created() {
        this.getCategories()
    }
}
</script>

<style lang="scss">
h3 {
  text-align: center;
  border: none;
  outline: none;
  background: none;
  width: 68%;
  max-width: 150px;
  background-color: #496583;
  padding: 16px;
  border-radius: 8px;
  color: #FFF;
  font-size: 14px;
  text-transform: uppercase;
  margin-bottom: 8px;
  transition: 0.4s;
}

.flex-container {
  display: flex;
}

.flex-child {
  flex: 1;
  border: 2px solid #496583;
  padding-left: 20px;
  width: 40%;
  max-width: 1500px;
}  

.flex-child:first-child {
  margin-right: 20px;
  width: 40%;
  max-width: 200px;
} 

p {
  padding: 16px;
  margin-right: 20px;
  background-color: #42B883;
  color: #FFF;
  border-radius: 8px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  font-size: 16px;
}

.cat {
  cursor: pointer;
  text-transform: uppercase;
}
</style>