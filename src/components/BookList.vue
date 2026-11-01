<template>
  <div class="book-list">
    <BookCard 
      v-for="book in books" 
      :key="book.id" 
      :book="book"
      @toggle-fav="$emit('toggle-fav', $event)"
    >
      <!-- Injection du contenu dans le slot nommé 'actions' -->
      <template #actions>
        <button class="details-button" @click="actionExemple(book.id)">Détails</button>
      </template>
    </BookCard>
  </div>
</template>

<script>
import BookCard from './BookCard.vue';

export default {
  name: 'BookList',
  components: {
    BookCard
  },
  props: {
    // Déclaration et validation de la prop books (tableau requis)
    books: {
      type: Array,
      required: true
    }
  },
  emits: ['toggle-fav'],
  methods: {
    actionExemple(id) {
      console.log('Action déclenchée pour le livre ID:', id);
    }
  }
}
</script>