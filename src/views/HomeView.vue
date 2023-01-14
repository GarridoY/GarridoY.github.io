<script setup lang="ts">
import Terminal from '@/components/Terminal.vue';
</script>

<template>
  <main class="container title-container">
    <div class="row title-container-wrapper">
      <div class="title-header-container col">
        <div class="row">
          <h1 class="col-12">👋<br/>Hi there! I'm<br/><span class="title-name">Daniél Garrido</span></h1>
          <p class="col-12" style="font-size: 1.2rem;">Computer Scientist with a secret passion for sustainable development 🌱</p>
        </div>
      </div>
      <div class="title-image-container col">
        <Terminal />
      </div>
    </div>
  </main>
</template>
