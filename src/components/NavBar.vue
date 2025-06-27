<template>
  <nav class="navbar">
    <div class="navbar-content">
      <!-- Logo -->
      <div class="navbar-left">
        <h1 class="logo">VUEFLIX</h1>
        <ul class="nav-links">
          <li><a href="#" class="active">Accueil</a></li>
          <li><a href="#">Séries</a></li>
          <li><a href="#">Films</a></li>
          <li><a href="#">Nouveautés</a></li>
          <li><a href="#">Ma liste</a></li>
        </ul>
      </div>

      <!-- Actions -->
      <div class="navbar-right">
        <!-- Recherche -->
        <div class="search-box" :class="{ active: searchActive }">
          <svg
            @click="toggleSearch"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
            />
          </svg>
          <input
            v-if="searchActive"
            type="text"
            :value="recherche"
            @input="$emit('update:recherche', $event.target.value)"
            @blur="closeSearch"
            placeholder="Titres, personnes, genres"
            ref="searchInput"
          />
        </div>

        <!-- Notifications -->
        <div class="notification-icon">
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
              d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"
            />
          </svg>
          <span class="notification-badge">3</span>
        </div>

        <!-- Profil -->
        <div class="profile-menu" @click="$emit('toggle-menu')">
          <img src="https://picsum.photos/32/32?random=99" alt="Profile" />
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
              d="M19 9l-7 7-7-7"
            />
          </svg>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'NavBar',
  props: {
    recherche: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      searchActive: false,
    }
  },
  methods: {
    toggleSearch() {
      this.searchActive = !this.searchActive
      if (this.searchActive) {
        this.$nextTick(() => {
          this.$refs.searchInput?.focus()
        })
      }
    },
    closeSearch() {
      // Délai pour permettre le clic sur le résultat
      setTimeout(() => {
        if (!this.recherche) {
          this.searchActive = false
        }
      }, 200)
    },
  },
}
</script>

<style scoped>
.navbar {
  position: sticky;
  top: 0;
  background: linear-gradient(to bottom, rgba(0, 0, 0, 0.9) 0%, rgba(0, 0, 0, 0) 100%);
  z-index: 1000;
  transition: background-color 0.3s ease;
}

.navbar-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 4%;
}

.navbar-left {
  display: flex;
  align-items: center;
  gap: 40px;
}

.logo {
  color: var(--accent);
  font-size: 28px;
  font-weight: bold;
  letter-spacing: 2px;
  cursor: pointer;
}

.nav-links {
  display: flex;
  list-style: none;
  gap: 20px;
}

.nav-links a {
  color: var(--text-secondary);
  text-decoration: none;
  font-size: 14px;
  transition: color 0.3s ease;
}

.nav-links a:hover,
.nav-links a.active {
  color: var(--text-primary);
}

.navbar-right {
  display: flex;
  align-items: center;
  gap: 20px;
}

.search-box {
  display: flex;
  align-items: center;
  position: relative;
}

.search-box svg {
  width: 24px;
  height: 24px;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.search-box.active svg {
  transform: translateX(-200px);
}

.search-box input {
  position: absolute;
  right: 0;
  width: 200px;
  padding: 8px 40px 8px 12px;
  background: rgba(0, 0, 0, 0.8);
  border: 1px solid var(--text-secondary);
  color: var(--text-primary);
  outline: none;
  transition: all 0.3s ease;
}

.notification-icon {
  position: relative;
  cursor: pointer;
}

.notification-icon svg {
  width: 24px;
  height: 24px;
}

.notification-badge {
  position: absolute;
  top: -8px;
  right: -8px;
  background: var(--accent);
  color: white;
  font-size: 10px;
  padding: 2px 6px;
  border-radius: 10px;
}

.profile-menu {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
}

.profile-menu img {
  width: 32px;
  height: 32px;
  border-radius: 4px;
}

.profile-menu svg {
  width: 16px;
  height: 16px;
  transition: transform 0.3s ease;
}

.profile-menu:hover svg {
  transform: rotate(180deg);
}

/* Responsive */
@media (max-width: 768px) {
  .nav-links {
    display: none;
  }

  .navbar-content {
    padding: 15px 4%;
  }

  .logo {
    font-size: 20px;
  }
}
</style>
