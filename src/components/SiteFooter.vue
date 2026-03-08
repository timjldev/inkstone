<template>
  <footer class="site-footer">
    <span class="footer-name">Marguerite Luntungan</span>
    <span class="separator" aria-hidden="true">·</span>
    <a href="mailto:marguerite@inkstone.art" class="footer-link">marguerite@inkstone.art</a>
    <span class="separator" aria-hidden="true">·</span>
    <span class="footer-address">PO Box 377, Sutherland NSW 1499</span>
  </footer>
</template>

<style scoped>
.site-footer {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  gap: 0.5rem 0.75rem;
  padding: 1.25rem 2rem;
  border-top: 1px solid rgba(232, 224, 213, 0.12);
  font-size: 0.78rem;
  color: var(--color-text-muted);
  letter-spacing: 0.05em;
}

.footer-name {
  color: var(--color-text);
}

.separator {
  color: var(--color-accent);
}

.footer-link {
  color: var(--color-accent);
  text-decoration: none;
  transition: opacity 0.2s;
}

.footer-link:hover {
  opacity: 0.75;
}
</style>
