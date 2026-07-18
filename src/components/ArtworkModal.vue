<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  artwork: { type: Object, default: null },
})

defineEmits(['close', 'prev', 'next'])

const base = import.meta.env.BASE_URL

const scale = ref(1)
const tx = ref(0)
const ty = ref(0)
const interacting = ref(false)

let pinchStartDist = 0
let pinchStartScale = 1
let panOrigin = null
let lastTapTime = 0

watch(() => props.artwork, () => {
  scale.value = 1
  tx.value = 0
  ty.value = 0
})

function touchDistance(touches) {
  const dx = touches[0].clientX - touches[1].clientX
  const dy = touches[0].clientY - touches[1].clientY
  return Math.hypot(dx, dy)
}

function onTouchStart(e) {
  if (e.touches.length === 2) {
    interacting.value = true
    pinchStartDist = touchDistance(e.touches)
    pinchStartScale = scale.value
  } else if (e.touches.length === 1) {
    const now = Date.now()
    if (now - lastTapTime < 300) {
      scale.value = scale.value > 1 ? 1 : 2.5
      tx.value = 0
      ty.value = 0
    }
    lastTapTime = now
    if (scale.value > 1) {
      interacting.value = true
      panOrigin = { x: e.touches[0].clientX - tx.value, y: e.touches[0].clientY - ty.value }
    }
  }
}

function onTouchMove(e) {
  if (e.touches.length === 2) {
    e.preventDefault()
    const dist = touchDistance(e.touches)
    scale.value = Math.min(4, Math.max(1, pinchStartScale * (dist / pinchStartDist)))
    if (scale.value === 1) {
      tx.value = 0
      ty.value = 0
    }
  } else if (e.touches.length === 1 && panOrigin) {
    e.preventDefault()
    tx.value = e.touches[0].clientX - panOrigin.x
    ty.value = e.touches[0].clientY - panOrigin.y
  }
}

function onTouchEnd(e) {
  if (e.touches.length === 0) {
    interacting.value = false
    panOrigin = null
  }
}
</script>

<template>
  <Teleport to="body">
    <div v-if="artwork" class="overlay" @click.self="$emit('close')">
      <button class="nav-arrow nav-arrow-left" @click="$emit('prev')" aria-label="Previous artwork">&#8592;</button>
      <button class="nav-arrow nav-arrow-right" @click="$emit('next')" aria-label="Next artwork">&#8594;</button>
      <div class="modal">
        <button class="close-btn" @click="$emit('close')" aria-label="Close">&#10005;</button>
        <div class="modal-body">
          <p class="detail-title">{{ artwork.description }}</p>
          <div
            class="image-pane"
            @touchstart="onTouchStart"
            @touchmove="onTouchMove"
            @touchend="onTouchEnd"
            @touchcancel="onTouchEnd"
          >
            <img
              :src="`${base}media/${artwork.fullimage}`"
              :alt="artwork.description"
              class="full-img"
              :style="{
                transform: `translate(${tx}px, ${ty}px) scale(${scale})`,
                transition: interacting ? 'none' : 'transform 0.2s ease',
              }"
            />
          </div>
          <div class="detail-pane">
            <p class="detail-body">{{ artwork.expanded }}</p>
            <p class="inquiry-note">Prices available upon inquiry.</p>
            <a href="mailto:inkstone@gmail.com" class="inquiry-link">inkstone@gmail.com</a>
          </div>
        </div>
      </div>
    </div>
  </Teleport>
</template>

<style scoped>
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.85);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 100;
}

.modal {
  position: relative;
  width: 80vw;
  max-height: 80vh;
  background: #0e0e0e;
  border: 1px solid rgba(232, 224, 213, 0.12);
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.close-btn {
  position: absolute;
  top: 1rem;
  right: 1rem;
  background: none;
  border: none;
  color: var(--color-text-muted);
  font-size: 0.9rem;
  cursor: pointer;
  line-height: 1;
  z-index: 1;
  transition: color 0.2s;
}

.close-btn:hover {
  color: var(--color-accent);
}

.nav-arrow {
  position: fixed;
  top: 50%;
  transform: translateY(-50%);
  background: none;
  border: none;
  color: var(--color-text-muted);
  opacity: 0.5;
  font-size: 1.75rem;
  cursor: pointer;
  padding: 1rem;
  line-height: 1;
  z-index: 101;
  transition: color 0.2s, opacity 0.2s;
}

.nav-arrow:hover {
  color: var(--color-accent);
  opacity: 1;
}

.nav-arrow-left {
  left: 1rem;
}

.nav-arrow-right {
  right: 1rem;
}

.modal-body {
  display: grid;
  grid-template-columns: 1fr 280px;
  grid-template-rows: auto 1fr;
  grid-template-areas:
    'image title'
    'image detail';
  height: 80vh;
  max-height: 80vh;
}

.detail-title {
  grid-area: title;
  padding: 2.5rem 2rem 0;
  font-size: 0.85rem;
  color: var(--color-accent);
  letter-spacing: 0.06em;
  text-transform: uppercase;
}

.image-pane {
  grid-area: image;
  background: #000;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  touch-action: none;
}

.full-img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  display: block;
  touch-action: none;
  user-select: none;
  -webkit-user-drag: none;
}

.detail-pane {
  grid-area: detail;
  padding: 1.25rem 2rem 2.5rem;
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
  border-left: 1px solid rgba(232, 224, 213, 0.08);
  overflow-y: auto;
}

.detail-body {
  font-size: 0.9rem;
  color: var(--color-text-muted);
  line-height: 1.8;
}

.inquiry-note {
  font-size: 0.85rem;
  color: var(--color-text);
  margin-top: auto;
}

.inquiry-link {
  font-size: 0.82rem;
  color: var(--color-accent);
  text-decoration: none;
  letter-spacing: 0.04em;
  transition: opacity 0.2s;
}

.inquiry-link:hover {
  opacity: 0.75;
}

@media (max-width: 700px) {
  .modal {
    width: 94vw;
    max-height: 92vh;
  }

  .modal-body {
    grid-template-columns: 1fr;
    grid-template-rows: auto auto auto;
    grid-template-areas:
      'title'
      'image'
      'detail';
    height: auto;
    max-height: 92vh;
    overflow-y: auto;
  }

  .detail-title {
    padding: 1.25rem 1.25rem 0;
  }

  .image-pane {
    height: 45vh;
  }

  .detail-pane {
    border-left: none;
    border-top: 1px solid rgba(232, 224, 213, 0.08);
    padding: 1.25rem 1.25rem 2rem;
    overflow-y: visible;
  }

  .nav-arrow {
    font-size: 1.4rem;
    padding: 0.5rem;
  }
}
</style>
