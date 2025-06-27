<template>
  <div id="app">
    <!-- NavBar -->
    <NavBar v-model:recherche="recherche" @toggle-menu="menuOuvert = !menuOuvert" />

    <!-- Hero Section -->
    <HeroSection
      v-if="filmMiseEnAvant"
      :film="filmMiseEnAvant"
      :est-dans-ma-liste="estDansMaListe(filmMiseEnAvant.id)"
      @play="lancerLecture"
      @add-to-list="toggleMaListe"
      @show-info="afficherDetails"
    />

    <!-- Contenu principal -->
    <main class="main-content">
      <!-- Section Continuer à regarder -->
      <CategorieRow
        v-if="filmsEnCours.length > 0"
        titre="Continuer à regarder"
        :films="filmsEnCours"
        :ma-liste="maListe"
        @select="afficherDetails"
        @toggle-list="toggleMaListe"
        @play="lancerLecture"
      />

      <!-- Ma Liste -->
      <CategorieRow
        v-if="maListeFilms.length > 0"
        titre="Ma Liste"
        :films="maListeFilms"
        :ma-liste="maListe"
        @select="afficherDetails"
        @toggle-list="toggleMaListe"
        @play="lancerLecture"
      />

      <!-- Résultats de recherche -->
      <div v-if="recherche" class="search-results">
        <h2>Résultats pour "{{ recherche }}"</h2>
        <div class="films-grid">
          <FilmCard
            v-for="film in filmsRecherche"
            :key="film.id"
            :film="film"
            :est-dans-ma-liste="estDansMaListe(film.id)"
            @select="afficherDetails"
            @toggle-list="toggleMaListe"
            @play="lancerLecture"
          />
        </div>
        <p v-if="filmsRecherche.length === 0" class="no-results">Aucun résultat trouvé</p>
      </div>

      <!-- Catégories -->
      <template v-else>
        <CategorieRow
          v-for="categorie in categories"
          :key="categorie"
          :titre="categorie"
          :films="filmsByCategorie(categorie)"
          :ma-liste="maListe"
          @select="afficherDetails"
          @toggle-list="toggleMaListe"
          @play="lancerLecture"
        />
      </template>
    </main>

    <!-- Modal détails -->
    <FilmDetail
      v-if="filmSelectionne"
      :film="filmSelectionne"
      :est-dans-ma-liste="estDansMaListe(filmSelectionne.id)"
      @close="filmSelectionne = null"
      @play="lancerLecture"
      @toggle-list="toggleMaListe"
    />
  </div>
</template>

<script>
import NavBar from './components/NavBar.vue'
import HeroSection from './components/HeroSection.vue'
import CategorieRow from './components/CategorieRow.vue'
import FilmCard from './components/FilmCard.vue'
import FilmDetail from './components/FilmDetail.vue'
import { filmsData } from './data/films.js'

export default {
  name: 'App',
  components: {
    NavBar,
    HeroSection,
    CategorieRow,
    FilmCard,
    FilmDetail,
  },
  data() {
    return {
      films: filmsData,
      recherche: '',
      filmSelectionne: null,
      maListe: [],
      historiqueVisionnage: [],
      menuOuvert: false,
    }
  },
  computed: {
    filmMiseEnAvant() {
      // Prendre un film au hasard ou le plus populaire
      return this.films.find((f) => f.note >= 4.5) || this.films[0]
    },
    filmsRecherche() {
      if (!this.recherche) return []
      const rechercheLower = this.recherche.toLowerCase()
      return this.films.filter(
        (film) =>
          film.titre.toLowerCase().includes(rechercheLower) ||
          film.description.toLowerCase().includes(rechercheLower) ||
          film.categorie.toLowerCase().includes(rechercheLower),
      )
    },
    categories() {
      return [...new Set(this.films.map((f) => f.categorie))]
    },
    filmsEnCours() {
      return this.films.filter((f) => f.enCours && f.progression > 0)
    },
    maListeFilms() {
      return this.films.filter((f) => this.maListe.includes(f.id))
    },
  },
  methods: {
    filmsByCategorie(categorie) {
      return this.films.filter((f) => f.categorie === categorie)
    },
    estDansMaListe(filmId) {
      return this.maListe.includes(filmId)
    },
    toggleMaListe(film) {
      const index = this.maListe.indexOf(film.id)
      if (index > -1) {
        this.maListe.splice(index, 1)
      } else {
        this.maListe.push(film.id)
      }
      this.sauvegarderDonnees()
    },
    lancerLecture(film) {
      // Mettre à jour le film
      const filmIndex = this.films.findIndex((f) => f.id === film.id)
      if (filmIndex > -1) {
        this.films[filmIndex].enCours = true
        if (!this.films[filmIndex].progression) {
          this.films[filmIndex].progression = 0
        }
        // Simuler progression
        this.films[filmIndex].progression += 10
      }

      // Ajouter à l'historique
      if (!this.historiqueVisionnage.includes(film.id)) {
        this.historiqueVisionnage.push(film.id)
      }

      this.sauvegarderDonnees()
      alert(`Lecture de "${film.titre}" en cours...`)
    },
    afficherDetails(film) {
      this.filmSelectionne = film
    },
    sauvegarderDonnees() {
      localStorage.setItem('vueflix-ma-liste', JSON.stringify(this.maListe))
      localStorage.setItem('vueflix-historique', JSON.stringify(this.historiqueVisionnage))
      localStorage.setItem('vueflix-films', JSON.stringify(this.films))
    },
    chargerDonnees() {
      const maListeSauvee = localStorage.getItem('vueflix-ma-liste')
      const historiqueSauve = localStorage.getItem('vueflix-historique')
      const filmsSauves = localStorage.getItem('vueflix-films')

      if (maListeSauvee) this.maListe = JSON.parse(maListeSauvee)
      if (historiqueSauve) this.historiqueVisionnage = JSON.parse(historiqueSauve)
      if (filmsSauves) {
        const filmsSauvesData = JSON.parse(filmsSauves)
        // Merger les données sauvées avec les nouvelles données
        this.films = this.films.map((film) => {
          const filmSauve = filmsSauvesData.find((f) => f.id === film.id)
          return filmSauve ? { ...film, ...filmSauve } : film
        })
      }
    },
  },
  mounted() {
    this.chargerDonnees()
  },
}
</script>

<style>
:root {
  --bg-primary: #141414;
  --bg-secondary: #000000;
  --text-primary: #ffffff;
  --text-secondary: #b3b3b3;
  --accent: #e50914;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: var(--bg-primary);
  color: var(--text-primary);
  font-family: 'Helvetica Neue', Arial, sans-serif;
}

#app {
  min-height: 100vh;
}

.main-content {
  padding: 20px 4%;
  position: relative;
  z-index: 1;
}

.search-results {
  margin-bottom: 40px;
}

.search-results h2 {
  font-size: 24px;
  margin-bottom: 20px;
}

.films-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}

.no-results {
  color: var(--text-secondary);
  font-size: 18px;
  text-align: center;
  padding: 40px;
}

/* Animations */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.main-content > * {
  animation: fadeIn 0.6s ease-out;
}
</style>
