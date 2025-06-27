<template>
  <div class="film-card" @click="$emit('select', film)">
    <div class="film-image">
      <img :src="film.image" :alt="film.titre" />

      <!-- Barre de progression -->
      <div v-if="film.enCours && film.progression > 0" class="progress-bar">
        <div class="progress-fill" :style="{ width: film.progression + '%' }"></div>
      </div>

      <!-- Overlay au hover -->
      <div class="film-overlay">
        <div class="overlay-content">
          <h3>{{ film.titre }}</h3>
          <div class="film-meta">
            <span>{{ film.annee }}</span>
            <span>{{ film.duree }}</span>
          </div>
          <p class="film-description">{{ truncateDescription(film.description) }}</p>

          <div class="film-actions">
            <button class="action-btn play" @click.stop="$emit('play', film)">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                <path
                  fill-rule="evenodd"
                  d="M10 18a8 8 0 100-16 8 8 0 000 16zM9.555 7.168A1 1 0 008 8v4a1 1 0 001.555.832l3-2a1 1 0 000-1.664l-3-2z"
                  clip-rule="evenodd"
                />
              </svg>
            </button>
            <button
              class="action-btn"
              @click.stop="$emit('toggle-list', film)"
              :title="estDansMaListe ? 'Retirer de ma liste' : 'Ajouter à ma liste'"
            >
              <svg
                v-if="!estDansMaListe"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M12 4v16m8-8H4"
                />
              </svg>
              <svg
                v-else
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
                fill="currentColor"
              >
                <path
                  fill-rule="evenodd"
                  d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
                  clip-rule="evenodd"
                />
              </svg>
            </button>
            <div class="film-rating">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                <path
                  d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
                />
              </svg>
              {{ film.note }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FilmCard',
  props: {
    film: {
      type: Object,
      required: true,
    },
    estDansMaListe: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    truncateDescription(text) {
      return text.length > 100 ? text.substring(0, 100) + '...' : text
    },
  },
}
</script>

<style scoped>
.film-card {
  position: relative;
  cursor: pointer;
  transition: transform 0.3s ease;
  border-radius: 4px;
  overflow: hidden;
}

.film-card:hover {
  transform: scale(1.05);
  z-index: 10;
}

.film-image {
  position: relative;
  aspect-ratio: 2/3;
  overflow: hidden;
}

.film-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.progress-bar {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: rgba(255, 255, 255, 0.3);
}

.progress-fill {
  height: 100%;
  background: var(--accent);
  transition: width 0.3s ease;
}

.film-overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.9);
  opacity: 0;
  transition: opacity 0.3s ease;
  display: flex;
  align-items: flex-end;
}

.film-card:hover .film-overlay {
  opacity: 1;
}

.overlay-content {
  padding: 20px;
  width: 100%;
}

.overlay-content h3 {
  font-size: 16px;
  margin-bottom: 8px;
}

.film-meta {
  display: flex;
  gap: 12px;
  font-size: 12px;
  color: var(--text-secondary);
  margin-bottom: 8px;
}

.film-description {
  font-size: 12px;
  line-height: 1.4;
  color: var(--text-secondary);
  margin-bottom: 12px;
}

.film-actions {
  display: flex;
  align-items: center;
  gap: 8px;
}

.action-btn {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  border: 1px solid rgba(255, 255, 255, 0.5);
  background: rgba(42, 42, 42, 0.6);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
}

.action-btn:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: white;
}

.action-btn.play {
  background: white;
  color: black;
  border-color: white;
}

.action-btn.play:hover {
  background: rgba(255, 255, 255, 0.8);
}

.action-btn svg {
  width: 16px;
  height: 16px;
}

.film-rating {
  margin-left: auto;
  display: flex;
  align-items: center;
  gap: 4px;
  font-size: 12px;
  color: #fbbf24;
}

.film-rating svg {
  width: 16px;
  height: 16px;
}
</style>
