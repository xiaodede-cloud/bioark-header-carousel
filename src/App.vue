<script setup lang="ts">
import HeroCarousel from './components/HeroCarousel.vue'
</script>

<template>
  <div class="page">
    <header class="page__topbar">
      <div class="page__brand">
        <img class="page__logo" src="https://www.bioarktech.com/images/BioArk-Logo-Circle-1-300x300.png" alt="BioArk logo" />
        <span class="page__brand-name">BioArk Technologies</span>
      </div>
      <nav class="page__nav">
        <a href="#" class="page__link">Products</a>
        <a href="#" class="page__link">Services</a>
        <a href="#" class="page__link">Design</a>
        <a href="#" class="page__link">Blog</a>
        <a href="#" class="page__link">Investors</a>
        <a href="#" class="page__link">Why BioArk</a>
      </nav>
      <button class="page__cta">Request Quote</button>
    </header>
    <main>
      <HeroCarousel />
    </main>
  </div>
</template>

<style scoped>
.page {
  min-height: 100vh;
  background-color: #f5f7fb;
  font-family: 'Inter', 'Segoe UI', sans-serif;
  color: #0f172a;
}

.page__topbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 72px;
  padding: 0 5vw;
  display: flex;
  align-items: center;
  justify-content: space-between;
  z-index: 5;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid rgba(148, 163, 184, 0.15);
  box-shadow: 0 12px 30px rgba(15, 23, 42, 0.05);
}

.page__brand {
  display: inline-flex;
  align-items: center;
  gap: 12px;
  color: #0f172a;
}

.page__logo {
  width: 40px;
  height: 40px;
}

.page__brand-name {
  font-weight: 600;
  font-size: 1.1rem;
}

.page__nav {
  display: flex;
  gap: 24px;
}

.page__link {
  color: #475569;
  text-decoration: none;
  font-size: 0.95rem;
  transition: color 0.2s ease;
}

.page__link:hover {
  color: #0f172a;
}

.page__cta {
  padding: 0.55rem 1.2rem;
  border-radius: 9999px;
  border: none;
  background-color: #1d4ed8;
  color: white;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.page__cta:hover {
  background-color: #2563eb;
}

main {
  margin-top: 72px;
}

@media (max-width: 960px) {
  .page__nav {
    display: none;
  }
}
</style>

<style>
html,
body {
  margin: 0;
  background-color: #f5f7fb;
}
</style>
