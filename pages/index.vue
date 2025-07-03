<template>
    <v-container>
        <v-row>
            <v-col cols="12" sm="6" md="4">
                <h1>Nossos produtos</h1>
            </v-col>
        </v-row>
        <v-row>
            <v-col class="d-flex" v-for="(img, i) in images" :key="i" cols="12" sm="6" md="4">
                <img :src=image alt="" class="w-100 h-100 image-content">
            </v-col>
        </v-row>
    </v-container>
</template>

<script setup lang="ts">
import image from '@/assets/images/IMG-20250702-WA0001.jpg'
import image1 from '@/assets/images/Imagem do WhatsApp de 2025-07-02 à(s) 22.34.20_2a36dbd7.jpg'
import image3 from '@/assets/images/Imagem do WhatsApp de 2025-07-02 à(s) 22.34.26_e6a86426.jpg'

const images = ref([image, image1, image3])
</script>

<style>
.image-content {
    border-radius: 12px;
}
</style>