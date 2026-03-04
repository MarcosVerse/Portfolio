<template>
  <CursorEffect />
  <NavBar />
  <HeroSection />
  <TechSection />
  <ProjectsSection />
</template>

<script setup>

import { useScrollReveal } from './composables/useScrollReveal.js'
useScrollReveal()


import NavBar from './components/NavBar.vue'
import HeroSection from './components/HeroSection.vue'
import CursorEffect from './components/CursorEffect.vue'
import TechSection from './components/TechSection.vue';
import ProjectsSection from './components/ProjectsSection.vue';


</script>