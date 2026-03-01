<template>
  <section class="hero">
    <h1 class="hero-title">INKSTONE <span>by Margueritte</span></h1>

    <div class="photo-placeholder">
      <img src="/marguerite.avif" alt="Margueritte" class="hero-photo" />
    </div>

    <p class="intro">
      Inkstone is an atelier dedicated to the contemplative arts — ink, charcoal, and pigment
      rendered on paper, linen, and stone. Each work emerges from a practice of stillness,
      bearing the traces of time and the texture of the hand. Originals and limited editions
      are available for private collectors worldwide.
    </p>
  </section>
</template>

<style scoped>
.hero {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 4rem 2rem 3rem;
  min-height: 65vh;
  text-align: center;
  gap: 2rem;
}

.hero-title {
  font-size: clamp(2rem, 6vw, 4rem);
  font-weight: 300;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: var(--color-text);
}

.hero-title span {
  display: block;
  font-size: 0.45em;
  letter-spacing: 0.3em;
  color: var(--color-accent);
  margin-top: 0.4em;
}

.photo-placeholder {
  width: 100%;
  max-width: 700px;
  aspect-ratio: 16 / 9;
  background: var(--color-surface);
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(232, 224, 213, 0.1);
}

.hero-photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.intro {
  max-width: 560px;
  color: var(--color-text-muted);
  font-size: 0.95rem;
  line-height: 1.8;
}
</style>
