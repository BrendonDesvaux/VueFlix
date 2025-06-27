<template>
  <div class="film-detail-modal" @click="$emit('close')">
    <div class="modal-content" @click.stop>
      <button class="close-btn" @click="$emit('close')">
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
            d="M6 18L18 6M6 6l12 12"
          />
        </svg>
      </button>

      <!-- Header avec image -->
      <div class="detail-header" :style="{ backgroundImage: `url(${film.banniere})` }">
        <div class="header-gradient"></div>
        <div class="header-content">
          <h1>{{ film.titre }}</h1>
          <div class="header-actions">
            <button class="btn btn-primary" @click="$emit('play', film)">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                <path
                  fill-rule="evenodd"
                  d="M10 18a8 8 0 100-16 8 8 0 000 16zM9.555 7.168A1 1 0 008 8v4a1 1 0 001.555.832l3-2a1 1 0 000-1.664l-3-2z"
                  clip-rule="evenodd"
                />
              </svg>
              Lecture
            </button>
            <button
              class="btn btn-icon"
              @click="$emit('toggle-list', film)"
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
          </div>
        </div>
      </div>

      <!-- Informations -->
      <div class="detail-info">
        <div class="info-main">
          <div class="info-meta">
            <span class="meta-year">{{ film.annee }}</span>
            <span class="meta-duration">{{ film.duree }}</span>
            <span class="meta-rating">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                <path
                  d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
                />
              </svg>
              {{ film.note }}/5
            </span>
          </div>

          <p class="info-description">{{ film.description }}</p>

          <div class="info-details">
            <p><strong>Catégorie:</strong> {{ film.categorie }}</p>
            <p v-if="film.realisateur"><strong>Réalisateur:</strong> {{ film.realisateur }}</p>
            <p v-if="film.acteurs"><strong>Acteurs:</strong> {{ film.acteurs }}</p>
          </div>
        </div>

        <!-- Section Episodes pour les séries -->
        <div v-if="film.episodes" class="episodes-section">
          <h3>Épisodes</h3>
          <div class="episodes-list">
            <div v-for="(episode, index) in film.episodes" :key="index" class="episode-item">
              <div class="episode-number">{{ index + 1 }}</div>
              <div class="episode-info">
                <h4>{{ episode.titre }}</h4>
                <p>{{ episode.duree }}</p>
              </div>
              <button class="episode-play" @click="$emit('play', film)">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                  <path
                    fill-rule="evenodd"
                    d="M10 18a8 8 0 100-16 8 8 0 000 16zM9.555 7.168A1 1 0 008 8v4a1 1 0 001.555.832l3-2a1 1 0 000-1.664l-3-2z"
                    clip-rule="evenodd"
                  />
                </svg>
              </button>
            </div>
          </div>
        </div>

        <!-- Films similaires -->
        <div class="similar-section">
          <h3>Contenu similaire</h3>
          <div class="similar-grid">
            <div v-for="i in 4" :key="i" class="similar-item">
              <img
                :src="`https://picsum.photos/200/300?random=${film.id + i}`"
                alt="Film similaire"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FilmDetail',
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
  mounted() {
    document.body.style.overflow = 'hidden'
  },
  beforeUnmount() {
    document.body.style.overflow = ''
  },
}
</script>

<style scoped>
.film-detail-modal {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.8);
  z-index: 2000;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  overflow-y: auto;
}

.modal-content {
  background: var(--bg-secondary);
  border-radius: 8px;
  max-width: 900px;
  width: 100%;
  max-height: 90vh;
  overflow-y: auto;
  position: relative;
}

.close-btn {
  position: absolute;
  top: 20px;
  right: 20px;
  background: rgba(0, 0, 0, 0.6);
  border: none;
  color: white;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 10;
  transition: background 0.3s ease;
}

.close-btn:hover {
  background: rgba(0, 0, 0, 0.8);
}

.close-btn svg {
  width: 24px;
  height: 24px;
}

.detail-header {
  position: relative;
  height: 400px;
  background-size: cover;
  background-position: center;
}

.header-gradient {
  position: absolute;
  inset: 0;
  background: linear-gradient(to bottom, transparent 0%, rgba(0, 0, 0, 0.8) 100%);
}

.header-content {
  position: absolute;
  bottom: 40px;
  left: 40px;
  right: 40px;
}

.header-content h1 {
  font-size: 36px;
  margin-bottom: 20px;
}

.header-actions {
  display: flex;
  gap: 12px;
}

.btn {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 12px 24px;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn svg {
  width: 20px;
  height: 20px;
}

.btn-primary {
  background: white;
  color: black;
}

.btn-primary:hover {
  background: rgba(255, 255, 255, 0.8);
}

.btn-icon {
  width: 48px;
  height: 48px;
  padding: 0;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(42, 42, 42, 0.6);
  border: 2px solid rgba(255, 255, 255, 0.5);
  color: white;
}

.btn-icon:hover {
  background: rgba(42, 42, 42, 0.8);
  border-color: white;
}

.detail-info {
  padding: 40px;
}

.info-meta {
  display: flex;
  align-items: center;
  gap: 20px;
  margin-bottom: 20px;
  font-size: 14px;
  color: var(--text-secondary);
}

.meta-rating {
  display: flex;
  align-items: center;
  gap: 4px;
  color: #fbbf24;
}

.meta-rating svg {
  width: 16px;
  height: 16px;
}

.info-description {
  font-size: 16px;
  line-height: 1.6;
  margin-bottom: 24px;
}

.info-details p {
  margin-bottom: 8px;
  color: var(--text-secondary);
}

.info-details strong {
  color: var(--text-primary);
}

.episodes-section,
.similar-section {
  margin-top: 40px;
}

.episodes-section h3,
.similar-section h3 {
  font-size: 24px;
  margin-bottom: 20px;
}

.episodes-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.episode-item {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 16px;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 4px;
  transition: background 0.3s ease;
}

.episode-item:hover {
  background: rgba(255, 255, 255, 0.1);
}

.episode-number {
  font-size: 24px;
  font-weight: bold;
  color: var(--text-secondary);
}

.episode-info {
  flex: 1;
}

.episode-info h4 {
  font-size: 16px;
  margin-bottom: 4px;
}

.episode-info p {
  font-size: 14px;
  color: var(--text-secondary);
}

.episode-play {
  background: none;
  border: none;
  color: white;
  cursor: pointer;
  padding: 8px;
}

.episode-play svg {
  width: 32px;
  height: 32px;
}

.similar-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 16px;
}

.similar-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 4px;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.similar-item:hover img {
  transform: scale(1.05);
}

/* Scrollbar styling */
.modal-content::-webkit-scrollbar {
  width: 8px;
}

.modal-content::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.1);
}

.modal-content::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.3);
  border-radius: 4px;
}
</style>
