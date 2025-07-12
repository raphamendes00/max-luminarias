<template>
  <div id="app">
    <header class="custom-app-bar">
      <div class="custom-app-bar-content">
        <!-- Logo -->
        <div class="logo-container">
          <img
            src="/assets/svg/site-removebg-preview (1).svg"
            alt="Max-Lumer-Logo"
            class="logo"
          />
        </div>
        <!-- Navigation Buttons -->
        <nav class="nav-btns">
          <router-link
            v-for="item in navItems"
            :key="item.title"
            :to="item.path"
            class="nav-btn"
          >
            {{ item.title }}
          </router-link>
        </nav>
      </div>
    </header>

    <main class="main-content">
      <router-view></router-view>
    </main>

    <footer class="custom-footer">
      <span>© 2025 Max Luminárias</span>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const navItems = ref([
  { title: 'Home', path: '/' },
  { title: 'Sobre Nós', path: '/sobre-nos' },
  { title: 'Contato', path: '/contato' },
]);
</script>

<style scoped>
.custom-app-bar {
  background: #263238;
  height: 110px;
  display: flex;
  align-items: center;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
  width: 100%;
  position: sticky;
  top: 0;
  z-index: 10;
}
.custom-app-bar-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 32px;
  height: 100%;
}
.logo-container {
  display: flex;
  align-items: center;
  height: 100%;
}
.logo {
  max-width: 220px;
  height: 140px;
  object-fit: contain;
  transition: max-width 0.2s, height 0.2s;
}
.nav-btns {
  display: flex;
  gap: 24px;
}
.nav-btn {
  color: #fff;
  background: transparent;
  border: none;
  font-size: 1.1rem;
  font-weight: 500;
  letter-spacing: 1px;
  text-decoration: none;
  padding: 8px 16px;
  border-radius: 6px;
  transition: background 0.2s;
}
.nav-btn:hover {
  background: rgba(255,255,255,0.08);
}
.main-content {
  margin-top: 24px;
  min-height: 70vh;
}
.custom-footer {
  background: #263238;
  color: #fff;
  text-align: center;
  padding: 16px 0;
  margin-top: 32px;
}

/* Responsive styles */
@media (max-width: 768px) {
  .custom-app-bar {
    height: auto;
    padding: 0;
  }
  .custom-app-bar-content {
    flex-direction: column;
    align-items: flex-start;
    padding: 8px 12px;
    height: auto;
  }
  .logo {
    max-width: 150px;
    height: 90px;
    margin-bottom: 8px;
  }
  .nav-btns {
    width: 100%;
    justify-content: flex-start;
    gap: 12px;
    flex-wrap: wrap;
  }
  .nav-btn {
    font-size: 1rem;
    padding: 6px 10px;
  }
}

@media (max-width: 480px) {
  .logo {
    max-width: 100px;
    height: 60px;
  }
  .nav-btns {
    gap: 8px;
  }
  .nav-btn {
    font-size: 0.95rem;
    padding: 5px 8px;
  }
}
</style>