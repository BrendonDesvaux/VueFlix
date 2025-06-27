<template>
  <section class="categorie-row">
    <h2 class="categorie-title">{{ titre }}</h2>
    <div class="films-container" ref="container">
      <button v-if="canScrollLeft" class="scroll-btn scroll-left" @click="scroll('left')">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M15 19l-7-7 7-7"
          />
        </svg>
      </button>

      <div class="films-wrapper" ref="wrapper">
        <FilmCard
          v-for="film in films"
          :key="film.id"
          :film="film"
          :est-dans-ma-liste="maListe.includes(film.id)"
          @select="$emit('select', film)"
          @toggle-list="$emit('toggle-list', film)"
          @play="$emit('play', film)"
        />
      </div>

      <button v-if="canScrollRight" class="scroll-btn scroll-right" @click="scroll('right')">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
        </svg>
      </button>
    </div>
  </section>
</template>

<script>
import FilmCard from './FilmCard.vue'

export default {
  name: 'CategorieRow',
  components: {
    FilmCard,
  },
  props: {
    titre: {
      type: String,
      required: true,
    },
    films: {
      type: Array,
      required: true,
    },
    maListe: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      canScrollLeft: false,
      canScrollRight: true,
    }
  },
  mounted() {
    this.checkScroll()
    this.$refs.wrapper.addEventListener('scroll', this.checkScroll)
  },
  beforeUnmount() {
    this.$refs.wrapper?.removeEventListener('scroll', this.checkScroll)
  },
  methods: {
    scroll(direction) {
      const wrapper = this.$refs.wrapper
      const scrollAmount = wrapper.clientWidth * 0.8

      if (direction === 'left') {
        wrapper.scrollBy({ left: -scrollAmount, behavior: 'smooth' })
      } else {
        wrapper.scrollBy({ left: scrollAmount, behavior: 'smooth' })
      }
    },
    checkScroll() {
      const wrapper = this.$refs.wrapper
      if (!wrapper) return

      this.canScrollLeft = wrapper.scrollLeft > 0
      this.canScrollRight = wrapper.scrollLeft < wrapper.scrollWidth - wrapper.clientWidth - 10
    },
  },
}
</script>

<style scoped>
.categorie-row {
  margin-bottom: 40px;
}

.categorie-title {
  font-size: 20px;
  font-weight: 600;
  margin-bottom: 16px;
  padding: 0 4%;
}

.films-container {
  position: relative;
  padding: 0 4%;
}

.films-wrapper {
  display: flex;
  gap: 8px;
  overflow-x: auto;
  scroll-behavior: smooth;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.films-wrapper::-webkit-scrollbar {
  display: none;
}

.scroll-btn {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 60px;
  background: rgba(0, 0, 0, 0.8);
  border: none;
  color: white;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.3s ease;
  z-index: 5;
}

.films-container:hover .scroll-btn {
  opacity: 1;
}

.scroll-left {
  left: 0;
  background: linear-gradient(to right, rgba(0, 0, 0, 0.8), transparent);
}

.scroll-right {
  right: 0;
  background: linear-gradient(to left, rgba(0, 0, 0, 0.8), transparent);
}

.scroll-btn svg {
  width: 32px;
  height: 32px;
}

.scroll-btn:hover {
  background: rgba(0, 0, 0, 0.9);
}

/* Film cards sizing */
.films-wrapper > :deep(*) {
  flex: 0 0 200px;
}

@media (max-width: 1200px) {
  .films-wrapper > :deep(*) {
    flex: 0 0 180px;
  }
}

@media (max-width: 768px) {
  .films-wrapper > :deep(*) {
    flex: 0 0 150px;
  }

  .scroll-btn {
    display: none;
  }
}
</style>
