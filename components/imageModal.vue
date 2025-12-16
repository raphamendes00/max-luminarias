<template>
  <!-- Usamos v-dialog em fullscreen para cobrir a tela -->
  <v-dialog v-model="open" persistent hide-overlay fullscreen>
    <div class="modal-backdrop" @click.self="close">

      <!-- Close button -->
      <v-btn
        icon
        class="close-btn"
        @click="close"
        aria-label="Fechar"
      >
        <v-icon>mdi-close</v-icon>
      </v-btn>

      <!-- Prev button -->
      <v-btn icon class="nav-btn left" @click.stop="previous" aria-label="Anterior">
        <v-icon>mdi-chevron-left</v-icon>
      </v-btn>

      <!-- Next button -->
      <v-btn icon class="nav-btn right" @click.stop="next" aria-label="Próximo">
        <v-icon>mdi-chevron-right</v-icon>
      </v-btn>

      <!-- Image container (centralizado) -->
      <div
        class="image-wrapper"
        ref="imageWrapper"
        @wheel.prevent="onWheel"
        @touchstart.passive="onTouchStart"
        @touchmove.passive="onTouchMove"
        @touchend="onTouchEnd"
      >
        <img
          :src="current?.url ?? ''"
          :alt="current?.fileName ?? 'image'"
          class="modal-image"
          :style="imageStyle"
          @click.stop="toggleZoom"
          @dblclick.stop="resetZoom"
          draggable="false"
        />
      </div>
    </div>
  </v-dialog>
</template>

<script setup lang="ts">
import { ref, computed, watch, onMounted, onBeforeUnmount } from 'vue'

/**
 * Props:
 * - modelValue: boolean - controla visibilidade (v-model)
 * - images: array de { fileName, url }
 * - initialIndex: número opcional
 */
const props = defineProps<{
  modelValue: boolean
  images: { fileName: string; url: string }[]
  initialIndex?: number
}>()

const emit = defineEmits<{
  (e: 'update:modelValue', v: boolean): void
  (e: 'close'): void
}>()

// controle interno do diálogo (sincroniza com o v-model do pai)
const open = ref<boolean>(props.modelValue ?? false)
watch(() => props.modelValue, (v) => (open.value = v))
watch(open, (v) => emit('update:modelValue', v))

// índice e imagem atual
const index = ref<number>(props.initialIndex ?? 0)
watch(
  () => props.initialIndex,
  (v) => {
    if (typeof v === 'number') index.value = v
  }
)

const current = computed(() => {
  return props.images && props.images.length > 0
    ? props.images[(index.value + props.images.length) % props.images.length]
    : null
})

// navegação circular
function next() {
  if (!props.images || props.images.length === 0) return
  index.value = (index.value + 1) % props.images.length
  resetZoom()
}
function previous() {
  if (!props.images || props.images.length === 0) return
  index.value = (index.value - 1 + props.images.length) % props.images.length
  resetZoom()
}

// fechar
function close() {
  open.value = false
  emit('close')
  resetZoom()
}

// ZOOM
const scale = ref<number>(1)
const minScale = 1
const maxScale = 3
const imageWrapper = ref<HTMLElement | null>(null)
const translate = ref({ x: 0, y: 0 }) // para drag quando zoomed (básico)

function setScale(v: number) {
  scale.value = Math.min(maxScale, Math.max(minScale, +v.toFixed(2)))
}

function onWheel(e: WheelEvent) {
  const delta = e.deltaY > 0 ? -0.12 : 0.12
  setScale(scale.value + delta)
}

// toggle por clique
function toggleZoom() {
  setScale(scale.value > 1 ? 1 : 2)
  if (scale.value === 1) {
    translate.value = { x: 0, y: 0 }
  }
}
function resetZoom() {
  scale.value = 1
  translate.value = { x: 0, y: 0 }
}

// estilo reativo aplicado na imagem
const imageStyle = computed(() => {
  return {
    transform: `scale(${scale.value}) translate(${translate.value.x}px, ${translate.value.y}px)`,
    transition: 'transform 220ms ease'
  }
})

// Touch swipe (simples): detecta direção horizontal
let touchStartX = 0
let touchStartY = 0
function onTouchStart(e: TouchEvent) {
  if (!e.touches || e.touches.length === 0) return
  touchStartX = e.touches[0].clientX
  touchStartY = e.touches[0].clientY
}
function onTouchMove(e: TouchEvent) {
  // você poderia implementar drag com touch aqui quando zoomed
}
function onTouchEnd(e: TouchEvent) {
  if (!e.changedTouches || e.changedTouches.length === 0) return
  const dx = e.changedTouches[0].clientX - touchStartX
  const dy = e.changedTouches[0].clientY - touchStartY
  // swipe horizontal com threshold
  const threshold = 50
  if (Math.abs(dx) > Math.abs(dy) && Math.abs(dx) > threshold) {
    if (dx < 0) next()
    else previous()
  }
}

// keyboard
function onKeydown(e: KeyboardEvent) {
  if (!open.value) return
  if (e.key === 'ArrowRight') next()
  if (e.key === 'ArrowLeft') previous()
  if (e.key === 'Escape') close()
}

onMounted(() => {
  window.addEventListener('keydown', onKeydown)
})
onBeforeUnmount(() => {
  window.removeEventListener('keydown', onKeydown)
})
</script>

<style scoped>
.modal-backdrop {
  width: 100%;
  height: 100%;
  background: rgba(8, 10, 12, 0.92);
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
  padding: 1.5rem;
}

/* central wrapper que limita tamanho da imagem */
.image-wrapper {
  max-width: 95%;
  max-height: 85%;
  display: flex;
  align-items: center;
  justify-content: center;
  touch-action: pan-y;
}

/* imagem responsiva */
.modal-image {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  cursor: zoom-in;
  user-select: none;
  -webkit-user-drag: none;
}

/* quando em zoom (scale > 1) o cursor muda */
.modal-image[style*="scale(2)"] {
  cursor: grab;
}

/* botões (usar ícones do mdi) */
.close-btn {
  position: absolute;
  top: 16px;
  right: 16px;
  z-index: 50;
  background: rgba(255,255,255,0.06);
  color: #fff;
}

.nav-btn {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  z-index: 50;
  background: rgba(255,255,255,0.04);
  color: #fff;
}

.nav-btn.left {
  left: 12px;
}

.nav-btn.right {
  right: 12px;
}

/* caption */
.caption {
  position: absolute;
  bottom: 18px;
  left: 50%;
  transform: translateX(-50%);
  color: rgba(255,255,255,0.85);
  font-size: 0.95rem;
  z-index: 40;
  max-width: 90%;
  text-align: center;
}

.modal-image {
  transition: transform 0.3s ease;
  max-width: 90vw;
  max-height: 90vh;
  object-fit: contain;
}

/* 🔍 Zoom menor no desktop */
.modal-image.zoomed {
  transform: scale(1.2);
}

/* 📱 Zoom maior no mobile */
@media (max-width: 600px) {
  .modal-image.zoomed {
    transform: scale(1.6);
  }
}

</style>
