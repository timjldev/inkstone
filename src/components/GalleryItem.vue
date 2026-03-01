<script setup>
defineProps({
  filename: { type: String, required: true },
  description: { type: String, required: true },
})
</script>

<template>
  <article class="gallery-item">
    <div class="img-wrapper">
      <img
        :src="`/media/${filename}`"
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
}

.img-wrapper {
  width: 200px;
  height: 160px;
  background: var(--color-surface);
  overflow: hidden;
  border: 1px solid rgba(232, 224, 213, 0.08);
}

.artwork-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.description {
  font-size: 0.78rem;
  color: var(--color-text-muted);
  line-height: 1.5;
}
</style>
