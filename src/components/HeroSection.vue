<script setup>
const base = import.meta.env.BASE_URL
</script>

<template>
  <section class="hero">
    <h1 class="hero-title">INKSTONE <span>by Marguerite</span></h1>

    <div class="photo-placeholder">
      <img :src="`${base}marguerite.avif`" alt="Marguerite" class="hero-photo" />
    </div>

    <p class="intro">
      Having lived in in Malaysia, Marguerite studied with a Taiwanese master in the Sumi-e discipline of inks on rice paper.<br/>

      Winning four international awards, she also studied in China with a Chinese living treasure, now deceased.<br/>

      Marguerite paints from her Northern Tablelands Garden studio, where she explores mixing Sumi-e with other painting styles.<br/>

      Marguerite paints with two very spoilt Rhodesian Ridgebacks.<br/>
    </p>
    <p class="intro-bold">
      <i>The tree knows its beauty, the artist tells its story.</i>
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
  max-width: 350px;
  background: var(--color-surface);
  border: 1px solid rgba(232, 224, 213, 0.1);
}

.hero-photo {
  width: 100%;
  height: auto;
  display: block;
}

.intro {
  max-width: 560px;
  color: var(--color-text-muted);
  font-size: 1.1rem;
  line-height: 1.8;
}
.intro-bold {
  max-width: 560px;
  color: var(--color-text-muted);
  font-size: 1.3rem;
  line-height: 1.8;
}
</style>
