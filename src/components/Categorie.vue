<template>
  <section>
    <h2>{{ categorieProp.name }}</h2>
    <div class="info">{{ infoMessage }}</div>
    <input type="text" v-model="categorieProp.name" placeholder="entrez le nom de la catégorie">
    <button v-on:click="updateCategorie">💾</button>
    <button v-on:click="deleteCategorie">🗑️</button>
  </section>
</template>

<script>
export default {
  name: 'Categorie',
  props: {
    categorieProp: Object,
    infoMessage: null
  },
  methods: {
    updateCategorie () {
      let APIURL;
      let HTTPmethod;
      if (this.categoryProd.id === undefined) {
        APIURL = 'https://127.0.0.1:8000/api/categories';
        HTTPmethod = 'POST';

      } else {
        APIURL = 'https://127.0.0.1:8000/api/categories/';
        HTTPmethod = 'PUT';
      }
      fetch(APIURL + this.categorieProp.id, {
        method: 'HTTPmethod',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          name: this.categorieProp.name
        })
      })
      .then((response) => {
        if (response.status == 200) {
          this.infoMessage = "✅ Modification de la catégorie effectuée";
        } else {
          this.infoMessage = "⚠ Une erreur est survenue lors de la modification de l'article'";
        }
        this.$emit('getCategoriesParent');
      })
    },
    deleteCategorie () {
      fetch('https://127.0.0.1:8000/api/categories/' + this.categorieProp.id, { method: 'DELETE'})
      .then((response) => {
        if (response.status === 204) {
          this.infoMessage = '✅ Article supprimée';
        } else {
          this.infoMessage = "⚠ Une erreur est survenue lors de la suppression de la catégorie";
        }
        this.$emit('getCategoriesParent');
      })
    }
  }
}
</script>

<style scoped>
h2 {
  color: darkolivegreen;
}
section {
    margin: 5px;
    border: 1px solid darkgrey;
    border-radius: 5px;
    background-color: lightgrey;
    padding: 20px 0;
}
</style>
