<template>
  <v-container class="min-h-[50vh] flex items-center justify-center pa-0">
    <v-row class="p-4">
      <!-- Instagram -->
      <v-col cols="12" sm="6">
        <v-card
          class="rounded-lg p-12 cursor-pointer hover:shadow-lg transition"
          @click="redirectToInstagram"
          elevation="2"
        >
          <div class="flex flex-col items-center text-center gap-5 p-4">
            <v-icon size="36" color="pink">mdi-instagram</v-icon>

            <h3 class="text-lg font-semibold">
              Acompanhe nosso Instagram
            </h3>

            <p class="text-sm text-gray-600">
              Veja novidades, lançamentos e inspirações dos nossos produtos.
            </p>

            <span class="text-sm font-medium text-pink-600">
              @maxlumerluminarias
            </span>
          </div>
        </v-card>
      </v-col>

      <!-- WhatsApp -->
      <v-col cols="12" sm="6">
        <v-card
          class="rounded-lg p-6 cursor-pointer hover:shadow-lg transition"
          elevation="2"
        >
          <div class="flex flex-col items-center text-center gap-5 p-4">
            <v-icon size="36" color="green">mdi-whatsapp</v-icon>

            <h3 class="text-lg font-semibold">
              Fale conosco no WhatsApp
            </h3>

            <p class="text-sm text-gray-600">
              Tire dúvidas e conheça mais produtos falando direto com a gente.
            </p>

            <span class="text-sm font-medium text-green-600">
              (31) 99440-6856
            </span>
          </div>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
    function redirectToInstagram() {
        window.open('https://www.instagram.com/maxlumerluminarias', '_blank')
    }
</script>