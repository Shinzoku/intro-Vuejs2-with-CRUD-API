<template>
  <div id="app">
    <h1>Catégories</h1>
    <select v-if="categories" v-model="selectedCategorie">
      <option value="">nouvelle catégorie</option>
      <option v-for="categorie in categories"
      v-bind:key="categorie['@id']"
      v-bind:value="categorie">{{ categorie.name }}</option>
    </select>
    <Categorie v-if="selectedCategorie"
    v-bind:key="selectedCategorie['@id']"
    v-bind:categorieProp="selectedCategorie"
    @getCategoriesParent="getCategories"/>
    <Categorie v-else v-bind:categorieProp="newCategorie"
    v-bind:key="newCategorie"
    @getCategoriesParent="getCategories"/>
  </div>
</template>

<script>
import Categorie from './components/Categorie'

export default {
  name: 'App',
  data () {
    return {
      categories: null,
      newCategorie: {},
      selectedCategorie: {}
    }
  },
  components: {
    Categorie
  },
  methods: {
    getCategories () {
      fetch('https://127.0.0.1:8000/api/categories', { method: 'GET' })
        .then(response => response.json())
        .then(responseJSON => {
          this.categories = responseJSON['hydra:member']
        })
    }
  },
  mounted () {
    this.getCategories()
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
