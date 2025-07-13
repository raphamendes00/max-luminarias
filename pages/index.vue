<template>
    <v-container>
        <v-row>
            <v-col cols="12" sm="6" md="4">
                <h1 class="title">Nossos produtos</h1>
            </v-col>
        </v-row>
        <v-row>
            <v-col class="d-flex flex-column align-center" v-for="(img, i) in images" :key="i" cols="12" sm="6" md="4">
                <img :src="img" alt="" class="w-100 h-100 image-content">
                <v-btn
                    @click="redirectToWhatsapp"
                    class="mt-2 d-flex align-center"
                    style="max-width: 200px; max-height: 36px; background-color: #263238;"
                    >
                    <img
                        src="/assets/images/whatsapp.png"
                        alt="WhatsApp"
                        style="width: 20px; height: 20px; margin-right: 8px;"
                    />
                    <span style="color: #fff; font-family: Arial, Helvetica, sans-serif;">Saiba mais</span>
                </v-btn>
            </v-col>
        </v-row>
    </v-container>
</template>

<script setup lang="ts">
import image from '@/assets/images/IMG-20250702-WA0001.jpg'
import image1 from '@/assets/images/Imagem do WhatsApp de 2025-07-02 à(s) 22.34.20_2a36dbd7.jpg'
import image3 from '@/assets/images/Imagem do WhatsApp de 2025-07-02 à(s) 22.34.26_e6a86426.jpg'

const images = ref([image, image1, image3])

function redirectToWhatsapp() {
    window.open('https://wa.me/5581999999999', '_blank');
}
</script>

<style>
.image-content {
    border-radius: 12px;
}

.title {
    font-size: 2rem;
    font-weight: bold;
    margin-bottom: 20px;
    font-family: Arial, Helvetica, sans-serif;
}
</style>