<!-- src/App.vue -->
<script setup>
import { RouterView, RouterLink } from 'vue-router'
</script>

<template>
  <header>
    <div class="container">
      <RouterLink to="/" class="logo">🍿 THAN's Mflix Tracker</RouterLink>
    </div>
  </header>

  <main>
    <RouterView />
  </main>
</template>

<style>
/* Reset & Global Vars */
:root {
  --primary: #e50914;
  --dark: #141414;
  --light: #f3f3f3;
  --gray: #808080;
}
body {
  margin: 0;
  font-family: 'Segoe UI', sans-serif;
  background-color: var(--dark);
  color: var(--light);
}
header {
  background-color: #000;
  padding: 1rem 0;
  border-bottom: 1px solid #333;
}
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}
.logo {
  color: var(--primary);
  font-size: 1.5rem;
  font-weight: bold;
  text-decoration: none;
}
</style>