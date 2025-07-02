<template>
  <v-app>
    <!-- Header with v-app-bar -->
    <v-app-bar :elevation="2" color="yellow" dark height="100">
      

      <!-- Logo, Title, and Search Field -->
      <v-container class="d-flex align-center justify-space-between px-4" style="max-width: 600px;">
        <div class="d-flex align-center">
          <!-- <v-img src="https://via.placeholder.com/40" class="mr-2" contain height="40" width="40"></v-img> -->
          <v-app-bar-title class="title">Max Lumer Luminárias</v-app-bar-title>
        </div>
      </v-container>
    </v-app-bar>

    <!-- Main Content -->
    <v-main>
      <v-container>
        <router-view></router-view>
      </v-container>
    </v-main>

    <!-- Footer -->
    <v-footer app>
      <v-container>
        <p>Desenvolvido por Raphael Mendes</p>
      </v-container>
    </v-footer>
  </v-app>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const drawer = ref(false);

const navItems = ref([
  { title: 'Home', path: '/' },
  { title: 'About', path: '/about' },
]);
</script>

<style scoped>
.title {
  font-size: 1.5rem;
  font-weight: bold;
}

.search-field {
  background-color: #fff;
}
</style>