<script setup>
defineProps({
  filename: { type: String, required: true },
  description: { type: String, required: true },
  fullimage: { type: String, required: true },
})

defineEmits(['select'])

const base = import.meta.env.BASE_URL
</script>

<template>
  <article class="gallery-item" @click="$emit('select')" role="button" tabindex="0" @keydown.enter="$emit('select')">
    <div class="img-wrapper">
      <img
        :src="`${base}media/${filename}`"
        :alt="description"
        class="artwork-img"
      />
    </div>
    <p class="description">{{ description }}</p>
  </article>
</template>

<style scoped>
.gallery-item {
  flex: 0 0 auto;
  width: 200px;
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
  cursor: pointer;
}

.img-wrapper {
  width: 200px;
  height: 160px;
  background: var(--color-surface);
  overflow: hidden;
  border: 1px solid rgba(232, 224, 213, 0.08);
  transition: border-color 0.2s;
}

.gallery-item:hover .img-wrapper {
  border-color: var(--color-accent);
}

.artwork-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transition: opacity 0.2s;
}

.gallery-item:hover .artwork-img {
  opacity: 0.85;
}

.description {
  font-size: 0.78rem;
  color: var(--color-text-muted);
  line-height: 1.5;
}
</style>
