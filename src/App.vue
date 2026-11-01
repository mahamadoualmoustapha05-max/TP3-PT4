<template>
  <div id="app" class="app-shell">
    <header class="hero">
      <div class="hero-copy">
        <p class="eyebrow">LA BIBLIOTHÈQUE QUI VOUS RESSEMBLE</p>
        <h1>Des histoires<br /><span>à garder près de soi.</span></h1>
        <p class="hero-description">
          Vos prochaines grandes aventures commencent sur cette étagère.
        </p>
      </div>
      <div class="hero-art" aria-hidden="true">
        <span class="sparkle sparkle-one">✦</span>
        <span class="sparkle sparkle-two">✳</span>
        <div class="book-stack">
          <span class="book book-one"></span>
          <span class="book book-two"></span>
          <span class="book book-three"></span>
        </div>
      </div>
    </header>

    <main class="content">
      <section class="form-panel" aria-labelledby="form-title">
        <div class="section-heading">
          <span class="section-icon" aria-hidden="true">＋</span>
          <div>
            <p class="eyebrow">UNE NOUVELLE PÉPITE ?</p>
            <h2 id="form-title">Ajouter un livre</h2>
          </div>
        </div>
        <AddBookForm @add-book="handleAddBook" />
      </section>

      <section class="collection" aria-labelledby="collection-title">
        <div class="collection-heading">
          <div>
            <p class="eyebrow">VOTRE PETIT UNIVERS</p>
            <h2 id="collection-title">La collection</h2>
          </div>
          <span class="book-count">{{ books.length }} livre<span v-if="books.length !== 1">s</span></span>
        </div>

        <p v-show="loading" class="loading-message">
          <span class="loading-dot"></span>
          On prépare votre bibliothèque...
        </p>
        <BookList
          v-show="!loading"
          :books="books"
          @toggle-fav="handleToggleFav"
        />
      </section>
    </main>

    <footer class="footer-note">Un bon livre, et le monde peut attendre. <span>✦</span></footer>
  </div>
</template>

<script>
import BookList from './components/BookList.vue';
import AddBookForm from './components/AddBookForm.vue';

// Données initiales d'exemple
const initialBooks = [
  { id: 1, title: 'Le Petit Prince', author: 'Antoine de Saint-Exupéry', isFav: false },
  { id: 2, title: 'L\'Étranger', author: 'Albert Camus', isFav: true },
  { id: 3, title: '1984', author: 'George Orwell', isFav: false }
];

export default {
  name: 'App',
  components: {
    BookList,
    AddBookForm
  },
  data() {
    return {
      books: [],
      loading: true
    };
  },
  mounted() {
    // Simulation d'un chargement réseau de 600ms
    setTimeout(() => {
      this.books = initialBooks;
      this.loading = false;
    }, 600);
  },
  methods: {
    handleToggleFav(bookId) {
      // Modification sécurisée de l'état dans le parent
      const book = this.books.find(b => b.id === bookId);
      if (book) {
        book.isFav = !book.isFav;
      }
    },
    handleAddBook(newBook) {
      this.books.push(newBook);
    }
  }
}
</script>

<style>
:root {
  font-family: 'Segoe UI', sans-serif;
  color: #242044;
  background: #fffaf4;
  font-synthesis: none;
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-optical-sizing: auto;
  font-weight: 400;
}

* {
  box-sizing: border-box;
}

body {
  min-width: 320px;
  min-height: 100vh;
  margin: 0;
  background:
    radial-gradient(ellipse at 4% 5%, rgba(255, 212, 129, 0.24), transparent 30rem),
    radial-gradient(ellipse at 95% 35%, rgba(210, 195, 255, 0.25), transparent 30rem),
    #fffaf4;
}

button,
input {
  font: inherit;
}

button {
  cursor: pointer;
}

.app-shell {
  width: min(1120px, calc(100% - 48px));
  margin: 0 auto;
}

.hero {
  position: relative;
  display: flex;
  min-height: 310px;
  align-items: center;
  justify-content: space-between;
  overflow: hidden;
  margin-top: 28px;
  padding: 48px clamp(28px, 7vw, 82px);
  border: 1px solid rgba(255, 255, 255, 0.5);
  border-radius: 30px;
  background: linear-gradient(118deg, #4b36a4 0%, #7356d8 57%, #9879ee 100%);
  box-shadow: 0 22px 55px rgba(74, 52, 151, 0.19);
  color: #fff;
}

.hero::after {
  position: absolute;
  right: -85px;
  bottom: -180px;
  width: 420px;
  height: 330px;
  border: 1px solid rgba(255, 255, 255, 0.13);
  border-radius: 50%;
  content: '';
}

.hero-copy,
.hero-art {
  position: relative;
  z-index: 1;
}

.eyebrow {
  margin: 0 0 9px;
  color: #8a73d8;
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.15em;
}

.hero .eyebrow {
  color: #e7dcff;
}

.hero h1,
.section-heading h2,
.collection-heading h2 {
  margin: 0;
  font-family: Georgia, serif;
  letter-spacing: -0.035em;
}

.hero h1 {
  font-size: clamp(2.4rem, 5vw, 4rem);
  line-height: 1.08;
}

.hero h1 span {
  color: #ffd879;
}

.hero-description {
  max-width: 390px;
  margin: 18px 0 0;
  color: #eee9ff;
  font-size: 0.98rem;
  line-height: 1.65;
}

.hero-art {
  width: 230px;
  height: 190px;
  flex: 0 0 230px;
}

.book-stack {
  position: absolute;
  right: 18px;
  bottom: 12px;
  display: flex;
  height: 134px;
  align-items: flex-end;
  gap: 8px;
  transform: rotate(-7deg);
}

.book {
  position: relative;
  display: block;
  width: 53px;
  border: 3px solid rgba(255, 255, 255, 0.76);
  border-radius: 7px 10px 10px 7px;
  box-shadow: 8px 13px 20px rgba(31, 19, 88, 0.24);
}

.book::after {
  position: absolute;
  top: 16px;
  right: 8px;
  left: 8px;
  height: 3px;
  border-radius: 4px;
  background: rgba(255, 255, 255, 0.76);
  box-shadow: 0 8px 0 rgba(255, 255, 255, 0.42);
  content: '';
}

.book-one {
  height: 119px;
  background: #f58372;
  transform: rotate(-8deg);
}

.book-two {
  height: 148px;
  background: #f3bf4f;
  transform: rotate(4deg);
}

.book-three {
  height: 128px;
  background: #55c5a2;
  transform: rotate(11deg);
}

.sparkle {
  position: absolute;
  color: #ffdc7f;
}

.sparkle-one {
  top: 5px;
  right: 40px;
  font-size: 2rem;
}

.sparkle-two {
  top: 53px;
  left: 6px;
  font-size: 1.35rem;
}

.content {
  display: grid;
  grid-template-columns: minmax(270px, 0.78fr) minmax(0, 1.5fr);
  align-items: start;
  gap: 26px;
  margin: 30px 0;
}

.form-panel,
.collection {
  padding: 28px;
  border: 1px solid #f0e9df;
  border-radius: 24px;
  background: rgba(255, 255, 255, 0.88);
  box-shadow: 0 12px 35px rgba(59, 41, 101, 0.06);
}

.section-heading {
  display: flex;
  align-items: center;
  gap: 14px;
  margin-bottom: 24px;
}

.section-heading h2,
.collection-heading h2 {
  font-size: 1.7rem;
}

.section-icon {
  display: grid;
  width: 44px;
  height: 44px;
  flex: 0 0 44px;
  place-items: center;
  border-radius: 14px;
  background: #fff0c8;
  color: #bd7910;
  font-size: 1.7rem;
}

.add-book-form {
  display: grid;
  gap: 16px;
}

.add-book-form > div {
  display: grid;
  gap: 7px;
}

.add-book-form label {
  color: #49445f;
  font-size: 0.82rem;
  font-weight: 700;
}

.add-book-form input {
  width: 100%;
  min-height: 46px;
  padding: 0 14px;
  border: 1px solid #e8e1f0;
  border-radius: 12px;
  outline: none;
  background: #fdfbff;
  color: #242044;
  transition: border-color 160ms ease, box-shadow 160ms ease;
}

.add-book-form input:focus {
  border-color: #8062dc;
  box-shadow: 0 0 0 4px rgba(128, 98, 220, 0.14);
}

.add-book-form > button {
  min-height: 48px;
  margin-top: 3px;
  border: 0;
  border-radius: 13px;
  background: linear-gradient(110deg, #6b4bc5, #8b67e8);
  box-shadow: 0 8px 17px rgba(107, 75, 197, 0.22);
  color: white;
  font-weight: 700;
  transition: transform 160ms ease, box-shadow 160ms ease;
}

.add-book-form > button:hover {
  transform: translateY(-2px);
  box-shadow: 0 11px 22px rgba(107, 75, 197, 0.3);
}

.collection-heading {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  margin-bottom: 23px;
}

.book-count {
  padding: 8px 13px;
  border-radius: 999px;
  background: #f0ebff;
  color: #684bb6;
  font-size: 0.78rem;
  font-weight: 700;
  white-space: nowrap;
}

.book-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(190px, 1fr));
  gap: 15px;
}

.book-card {
  position: relative;
  min-height: 190px;
  overflow: hidden;
  padding: 21px 18px 17px;
  border: 1px solid #efe9df;
  border-radius: 19px;
  background: linear-gradient(145deg, #fff 25%, #fff8e9 100%);
  transition: transform 180ms ease, box-shadow 180ms ease;
}

.book-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 13px 24px rgba(59, 41, 101, 0.11);
}

.book-card::before {
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
  height: 5px;
  background: #ef9b6b;
  content: '';
}

.book-card:nth-child(3n + 2)::before {
  background: #59bfa0;
}

.book-card:nth-child(3n)::before {
  background: #8d75df;
}

.book-card h3 {
  margin: 3px 0 8px;
  color: #302748;
  font-family: Georgia, serif;
  font-size: 1.2rem;
  line-height: 1.3;
}

.book-card p {
  min-height: 21px;
  margin: 0 0 17px;
  color: #777188;
  font-size: 0.82rem;
}

.book-card > button,
.details-button {
  min-height: 34px;
  padding: 0 11px;
  border: 1px solid #e6def5;
  border-radius: 10px;
  background: #f6f2ff;
  color: #654bb0;
  font-size: 0.75rem;
  font-weight: 700;
  transition: background 160ms ease, transform 160ms ease;
}

.book-card > button:hover,
.details-button:hover {
  transform: translateY(-1px);
  background: #eae2ff;
}

.details-button {
  margin-left: 7px;
  border-color: #f4e3bf;
  background: #fff5dc;
  color: #9a6715;
}

.details-button:hover {
  background: #ffedc2;
}

.loading-message {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #777188;
}

.loading-dot {
  width: 9px;
  height: 9px;
  border-radius: 50%;
  background: #8062dc;
  box-shadow: 0 0 0 5px #eee8ff;
  animation: pulse 900ms ease-in-out infinite alternate;
}

.footer-note {
  padding: 5px 0 24px;
  color: #827c91;
  font-size: 0.8rem;
  text-align: center;
}

.footer-note span {
  color: #e5a82f;
}

@keyframes pulse {
  to {
    opacity: 0.45;
    transform: scale(0.8);
  }
}

@media (max-width: 760px) {
  .hero {
    min-height: 280px;
    padding: 38px 30px;
  }

  .hero-art {
    width: 155px;
    flex-basis: 155px;
    transform: scale(0.82);
    transform-origin: right center;
  }

  .content {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 520px) {
  .app-shell {
    width: min(100% - 28px, 1120px);
  }

  .hero {
    min-height: 260px;
    margin-top: 14px;
    padding: 30px 23px;
    border-radius: 23px;
  }

  .hero h1 {
    font-size: 2.45rem;
  }

  .hero-description {
    max-width: 270px;
    font-size: 0.88rem;
  }

  .hero-art {
    position: absolute;
    right: 4px;
    bottom: -8px;
    opacity: 0.48;
    transform: scale(0.7);
    transform-origin: right bottom;
  }

  .form-panel,
  .collection {
    padding: 22px 19px;
    border-radius: 20px;
  }

  .book-list {
    grid-template-columns: 1fr;
  }
}
</style>