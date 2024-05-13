<script setup lang="ts">
import PDFPreviews from './views/PDFPreviews.vue'

const print = () => {
  window.print()
}
</script>

<template>
  <main>
    <button @click="print">Skriv ut skyltar</button>
    <PDFPreviews />
  </main>
</template>

<style scoped>
@media print {
  button {
    display: none;
  }
}

button {
  position: sticky;
  top: 1rem;
  left: 1rem;
  z-index: 1;
  padding: 0.5rem 1rem;
  font-size: small;
  color: white;
  border: none;
  border-radius: 0.25rem;
  cursor: pointer;
  background-color: #325b9a;
}
</style>
