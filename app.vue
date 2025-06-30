<template>
  <v-app>
      <div id="app">
        <header>
          <v-app-bar :elevation="22">
            <template>
              <v-app-bar-nav-icon></v-app-bar-nav-icon>
            </template>
          </v-app-bar>
        </header>
        <main>
          <router-view></router-view>
        </main>
        <footer>
          <p>Desenvolvido por Raphael Mendes</p>
        </footer>
      </div>
    </v-app>
</template>

<script setup lang="ts">
import index from './pages/index.vue';
</script>