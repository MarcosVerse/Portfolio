<template>
  <CursorEffect />
  <NavBar />
  <HeroSection />
  <AboutSection />
  <TechSection />
  <ProjectsSection />
  <ContactSection />
  <FooterBar />
</template>

<script setup>

import { useScrollReveal } from './composables/useScrollReveal.js'
useScrollReveal()


import NavBar from './components/NavBar.vue'
import HeroSection from './components/HeroSection.vue'
import CursorEffect from './components/CursorEffect.vue'
import TechSection from './components/TechSection.vue';
import ProjectsSection from './components/ProjectsSection.vue';
import AboutSection from './components/AboutSection.vue'
import ContactSection from './components/ContactSection.vue'
import FooterBar from './components/FooterBar.vue'


</script>