<script setup>
defineProps({
  artwork: { type: Object, default: null },
})

defineEmits(['close'])
</script>

<template>
  <Teleport to="body">
    <div v-if="artwork" class="overlay" @click.self="$emit('close')">
      <div class="modal">
        <button class="close-btn" @click="$emit('close')" aria-label="Close">&#10005;</button>
        <div class="modal-body">
          <div class="image-pane">
            <img
              :src="`/media/${artwork.fullimage}`"
              :alt="artwork.description"
              class="full-img"
            />
          </div>
          <div class="detail-pane">
            <p class="detail-description">{{ artwork.description }}</p>
            <p class="detail-body">
              This work is an original piece by Margueritte, rendered with meticulous attention
              to surface, tone, and gesture. Each piece is unique and bears the unmistakable
              character of the artist's hand.
            </p>
            <p class="inquiry-note">Prices available upon inquiry.</p>
            <a href="mailto:hello@inkstone.art" class="inquiry-link">hello@inkstone.art</a>
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

.modal-body {
  display: flex;
  height: 80vh;
  max-height: 80vh;
}

.image-pane {
  flex: 1 1 60%;
  background: #000;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.full-img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  display: block;
}

.detail-pane {
  flex: 0 0 280px;
  padding: 2.5rem 2rem;
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
  border-left: 1px solid rgba(232, 224, 213, 0.08);
  overflow-y: auto;
}

.detail-description {
  font-size: 0.85rem;
  color: var(--color-accent);
  letter-spacing: 0.06em;
  text-transform: uppercase;
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
</style>
