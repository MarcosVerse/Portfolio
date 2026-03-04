<template>
    <section id="projects" class="projects-section">
        <div class="projects-inner">
            <div class="reveal">
                <div class="section-label font-mono text-xs uppercase tracking-widest">// trabalhos</div>
                <h2>Projetos</h2>
            </div>
            <div class="projects-grid reveal">
                <ProjectCard v-for="project in projects" :key="project.num" :project="project" />
            </div>
        </div>
    </section>
</template>

<script setup>
import ProjectCard from './ProjectCard.vue'
import { projects } from '../data/projects.js'
</script>

<style scoped>
.projects-section {
    padding: 120px 48px;
}

.projects-inner {
    max-width: 1100px;
    margin: 0 auto;
}

.section-label {
    color: var(--accent);
    margin-bottom: 20px;
}

h2 {
    font-family: var(--sans);
    font-size: clamp(32px, 4vw, 48px);
    font-weight: 700;
    line-height: 1.05;
    letter-spacing: -0.02em;
    margin-bottom: 60px;
}

.projects-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 1px;
    border: 1px solid var(--border);
    overflow: hidden;
}

@media (max-width: 768px) {
    .projects-section {
        padding: 80px 24px;
    }

    .projects-grid {
        grid-template-columns: 1fr;
    }
}
</style>