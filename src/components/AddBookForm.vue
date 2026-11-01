<template>
  <form @submit.prevent="submitForm" class="add-book-form">
    <div>
      <label for="title">Titre :</label>
      <!-- Référence 'titreInput' ajoutée sur le champ -->
      <input 
        id="title"
        ref="titreInput" 
        v-model="title" 
        type="text" 
        placeholder="Titre du livre"
      />
    </div>
    <div>
      <label for="author">Auteur :</label>
      <input id="author" v-model="author" type="text" placeholder="Auteur" />
    </div>
    <button type="submit">Ajouter</button>
  </form>
</template>

<script>
export default {
  name: 'AddBookForm',
  data() {
    return {
      title: '',
      author: ''
    };
  },
  emits: ['add-book'],
  mounted() {
    // Le curseur se place automatiquement sur le champ titre à l'ouverture
    this.$refs.titreInput.focus();
  },
  methods: {
    submitForm() {
      if (!this.title.trim()) return;
      
      this.$emit('add-book', {
        id: Date.now(),
        title: this.title,
        author: this.author,
        isFav: false
      });

      this.title = '';
      this.author = '';
    }
  }
}
</script>