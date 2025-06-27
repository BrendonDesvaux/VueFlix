<template>
  <section class="hero-section" :style="backgroundStyle">
    <div class="hero-gradient"></div>
    <div class="hero-content">
      <h1 class="hero-title">{{ film.titre }}</h1>
      <div class="hero-meta">
        <span class="hero-year">{{ film.annee }}</span>
        <span class="hero-duration">{{ film.duree }}</span>
        <span class="hero-rating">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
            <path
              d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
            />
          </svg>
          {{ film.note }}/5
        </span>
      </div>
      <p class="hero-description">{{ film.description }}</p>
      <div class="hero-actions">
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
        <button class="btn btn-secondary" @click="$emit('show-info', film)">
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
              d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
            />
          </svg>
          Plus d'infos
        </button>
        <button
          class="btn btn-icon"
          @click="$emit('add-to-list', film)"
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
          <svg v-else xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
            <path
              fill-rule="evenodd"
              d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
              clip-rule="evenodd"
            />
          </svg>
        </button>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'HeroSection',
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
  computed: {
    backgroundStyle() {
      return {
        backgroundImage: `url(${this.film.banniere})`,
        backgroundSize: 'cover',
        backgroundPosition: 'center',
      }
    },
  },
}
</script>

<style scoped>
.hero-section {
  position: relative;
  height: 80vh;
  min-height: 500px;
  display: flex;
  align-items: center;
}

.hero-gradient {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to right,
    rgba(0, 0, 0, 0.8) 0%,
    rgba(0, 0, 0, 0.4) 50%,
    rgba(0, 0, 0, 0.1) 100%
  );
}

.hero-content {
  position: relative;
  z-index: 1;
  padding: 0 4%;
  max-width: 600px;
}

.hero-title {
  font-size: clamp(2rem, 5vw, 3.5rem);
  font-weight: bold;
  margin-bottom: 16px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.8);
}

.hero-meta {
  display: flex;
  align-items: center;
  gap: 20px;
  margin-bottom: 20px;
  font-size: 14px;
  color: var(--text-secondary);
}

.hero-rating {
  display: flex;
  align-items: center;
  gap: 4px;
  color: #fbbf24;
}

.hero-rating svg {
  width: 16px;
  height: 16px;
}

.hero-description {
  font-size: 18px;
  line-height: 1.6;
  margin-bottom: 32px;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.8);
}

.hero-actions {
  display: flex;
  align-items: center;
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

.btn-secondary {
  background: rgba(109, 109, 110, 0.7);
  color: white;
}

.btn-secondary:hover {
  background: rgba(109, 109, 110, 0.5);
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

/* Responsive */
@media (max-width: 768px) {
  .hero-section {
    height: 60vh;
  }

  .hero-description {
    font-size: 16px;
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }

  .btn {
    padding: 10px 20px;
    font-size: 14px;
  }
}
</style>
