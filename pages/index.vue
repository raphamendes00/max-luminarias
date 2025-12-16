<template>
  <v-container fluid class="pa-4 pa-sm-8"> <!-- opcional: tira padding ou deixa maior -->

    <!-- LINHA DO TÍTULO + BOTÃO -->
    <v-row class="mb-8">
      <v-col cols="12">
        <div class="d-flex justify-space-between align-center flex-wrap">
          <h1 class="text-h4 font-weight-bold">Nossos produtos</h1>

          <v-btn
            @click="redirectToWhatsapp"
            color="#263238"
            variant="flat"
            class="text-none"
            prepend-icon="mdi-whatsapp"
            size="large"
          >
            Saiba mais
          </v-btn>
        </div>
      </v-col>
    </v-row>

    <!-- GRID DAS IMAGENS -->
    <v-row>
        <v-col
            v-for="(img, i) in images"
            :key="img.fileName"
            cols="6"     
            sm="4"       
            md="3" 
            lg="3"
            xl="2"      
            class="d-flex justify-center"
        >
            <img
                :src="img.url"
                :alt="img.fileName"
                class="w-100 image-content"

                @click="openModal(i)"
            />
        </v-col>
    </v-row>
    <ImageModal
      v-model="showModal"
      :images="images"
      :initialIndex="selectedIndex"
      @close="onModalClose"
    />
  </v-container>
</template>

<script setup lang="ts">
import ImageModal from '~/components/imageModal.vue';

const modules = import.meta.glob('/assets/images/*.{png,jpg,jpeg,webp,svg,gif}', {
  eager: true,
  as: 'url'
})

const images = ref<{ fileName: string; url: string }[]>(
  Object.entries(modules).map(([path, url]) => {
    const fileName = path.split('/').pop() || path
    return { fileName, url: url as string }
  })
)

function redirectToWhatsapp() {
  window.open('https://wa.me/5531994406865', '_blank');
}

const showModal = ref(false)
const selectedIndex = ref(0)

function openModal(i: number) {
  selectedIndex.value = i
  showModal.value = true
}

function onModalClose() {
  // opcional: alguma lógica quando modal fecha
  // ex: console.log('modal fechado')
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