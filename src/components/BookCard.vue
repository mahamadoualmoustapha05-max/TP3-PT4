<template>
  <div class="book-card">
    <h3>{{ book.title }}</h3>
    <p>Auteur : {{ book.author }}</p>
    
    <!-- Bouton favori envoyant un événement $emit pour ne pas muter la prop -->
    <button @click="toggleFavorite">
      {{ book.isFav ? '★ Favori' : '☆ Ajouter aux favoris' }}
    </button>

    <!-- Slot nommé 'actions' après le badge/bouton favori -->
    <slot name="actions"></slot>
  </div>
</template>

<script>
export default {
  name: 'BookCard',
  props: {
    // Validation de la prop 'book'
    book: {
      type: Object,
      required: true,
      validator(value) {
        // Vérifie que la propriété title existe et n'est pas vide
        return value !== null &&
          typeof value === 'object' &&
          typeof value.title === 'string' &&
          value.title.trim() !== '';
      }
    }
  },
  methods: {
    toggleFavorite() {
      // Demande au parent de modifier l'état (flux à sens unique)
      this.$emit('toggle-fav', this.book.id);
    }
  }
}
</script>