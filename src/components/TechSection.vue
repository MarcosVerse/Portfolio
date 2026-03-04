<template>
    <section id="tech" class="tech-section">
        <div class="tech-inner">
            <div class="reveal">
                <div class="section-label font-mono text-xs uppercase tracking-widest">// stack</div>
                <h2>Tecnologias</h2>
            </div>
            <div class="tech-grid reveal">
                <TechItem v-for="tech in techs" :key="tech.name" :tech="tech" />
            </div>
        </div>
    </section>
</template>

<script setup>
import TechItem from './TechItem.vue'
import { techs } from '../data/techs.js'
</script>

<style scoped>
.tech-section {
    padding: 120px 48px;
    background: var(--surface);
    border-top: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
}

.tech-inner {
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

.tech-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
    gap: 1px;
    border: 1px solid var(--border);
    overflow: hidden;
}

@media (max-width: 768px) {
    .tech-section {
        padding: 80px 24px;
    }
}
</style>