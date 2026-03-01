<script setup>
import { ref, onMounted } from 'vue'
import GalleryItem from './GalleryItem.vue'
import ArtworkModal from './ArtworkModal.vue'

const artworks = ref([])
const strip = ref(null)
const selected = ref(null)

onMounted(async () => {
  const res = await fetch('/artworks.csv')
  const text = await res.text()
  const lines = text.split('\n').slice(1) // skip header
  artworks.value = lines
    .filter(line => line.trim())
    .map(line => {
      const parts = line.split(',')
      return {
        filename: parts[0].trim(),
        description: parts[1].trim(),
        fullimage: parts[2].trim(),
      }
    })
})

function scroll(dir) {
  strip.value.scrollBy({ left: dir * 240, behavior: 'smooth' })
}
</script>

<template>
  <section class="gallery-section">
    <div class="gallery-header">
      <h2 class="gallery-title">Explore the collection</h2>
      <p class="gallery-sub">Prices available upon inquiry</p>
    </div>
    <div class="scroll-wrapper">
      <button class="arrow arrow-left" @click="scroll(-1)" aria-label="Scroll left">&#8592;</button>
      <div class="scroll-strip" ref="strip">
        <GalleryItem
          v-for="art in artworks"
          :key="art.filename"
          :filename="art.filename"
          :description="art.description"
          :fullimage="art.fullimage"
          @select="selected = art"
        />
      </div>
      <button class="arrow arrow-right" @click="scroll(1)" aria-label="Scroll right">&#8594;</button>
    </div>
  </section>

  <ArtworkModal :artwork="selected" @close="selected = null" />
</template>

<style scoped>
.gallery-section {
  min-height: 280px;
  height: 33vh;
  display: flex;
  flex-direction: column;
  padding: 1.5rem 2rem;
  border-top: 1px solid rgba(232, 224, 213, 0.12);
  gap: 1rem;
}

.gallery-header {
  flex: 0 0 auto;
}

.gallery-title {
  font-size: 1rem;
  font-weight: 400;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--color-text);
}

.gallery-sub {
  font-size: 0.78rem;
  color: var(--color-text-muted);
  margin-top: 0.2rem;
  letter-spacing: 0.05em;
}

.scroll-wrapper {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  flex: 1;
  min-height: 0;
}

.scroll-strip {
  display: flex;
  gap: 1.25rem;
  overflow-x: auto;
  padding-bottom: 0.5rem;
  flex: 1;
  scrollbar-width: thin;
  scrollbar-color: var(--color-accent) transparent;
}

.scroll-strip::-webkit-scrollbar {
  height: 4px;
}

.scroll-strip::-webkit-scrollbar-thumb {
  background: var(--color-accent);
  border-radius: 2px;
}

.arrow {
  flex: 0 0 auto;
  background: none;
  border: none;
  color: var(--color-text-muted);
  font-size: 1.1rem;
  cursor: pointer;
  padding: 0.25rem;
  line-height: 1;
  transition: color 0.2s;
}

.arrow:hover {
  color: var(--color-accent);
}
</style>
