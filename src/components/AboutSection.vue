<template>
    <section id="about" class="about-section">
        <div class="about-left reveal">
            <div class="section-label font-mono text-xs uppercase tracking-widest">// sobre mim</div>
            <h2>Código como forma de pensar</h2>
            <div class="about-text font-mono text-sm">
                <p>
                    Desenvolvedor de software com experiência na construção de aplicações robustas e escaláveis.
                    Atuo tanto no frontend quanto no backend, conectando camadas com foco em performance,
                    clareza de arquitetura e boa experiência para o usuário.
                </p>

                <p>
                    Antes de escrever código, procuro entender bem o problema. Acredito que software de qualidade
                    é resultado de boas decisões, e boas decisões começam fazendo as perguntas certas.
                </p>

                <p>
                    Fora do código, gosto de estudar arquitetura de sistemas, explorar novas ferramentas
                    e otimizar meu ambiente de desenvolvimento para trabalhar de forma mais eficiente.
                </p>
            </div>
        </div>
        <div class="about-right reveal">
            <div class="timeline">
                <div class="timeline-item" v-for="(item, index) in timeline" :key="index"
                    :class="{ active: item.active }">
                    <div class="timeline-left">
                        <div class="timeline-dot">
                            <div class="dot-inner"></div>
                        </div>
                        <div class="timeline-line" v-if="index < timeline.length - 1"></div>
                    </div>
                    <div class="timeline-body">
                        <div class="timeline-meta">
                            <span class="timeline-year font-mono text-xs tracking-widest">{{ item.year }}</span>
                            <span class="timeline-badge font-mono text-xs" v-if="item.badge">{{ item.badge }}</span>
                        </div>
                        <div class="timeline-title">{{ item.title }}</div>
                        <div class="timeline-desc font-mono text-xs">{{ item.desc }}</div>
                        <div class="timeline-tags" v-if="item.tags">
                            <span class="tl-tag font-mono text-[10px]" v-for="tag in item.tags" :key="tag">{{ tag
                            }}</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<script setup>
import { useScrollReveal } from '../composables/useScrollReveal.js'

useScrollReveal()

const timeline = [
    {
        year: 'Mai. 2024',
        badge: 'atual',
        active: true,
        title: 'Estágio — Desenvolvimento Full Stack na Celepar',
        desc: 'Atuação no Projeto Colheita Urbana (SEAB) com desenvolvimento de interfaces em Vue.js, consumo de APIs REST e manutenção de backend em Java com Quarkus. Responsável por CRUDs, modelagem de dados (MER), engenharia reversa de banco de dados e documentação técnica de sistemas legados. Configuração de produtos como LimeSurvey e suporte a soluções de IAM (Identity & Access Management).',
        tags: ['Vue.js', 'Java', 'Quarkus', 'JavaScript', 'IAM'],
    },
    {
        year: 'Mar. 2024 — Mai. 2024',
        badge: null,
        active: false,
        title: 'Estágio — Suporte de TI na Celepar',
        desc: 'Início da trajetória profissional com atuação em suporte técnico. Após dois meses, migração para a área de desenvolvimento, onde permanece até hoje.',
        tags: ['Suporte TI', 'Redes', 'Help Desk'],
    },
    {
        year: 'Mar. 2024',
        badge: null,
        active: false,
        title: 'Início da Faculdade',
        desc: 'Ingresso no curso de Análise e Desenvolvimento de Sistemas. Primeiros contatos com lógica de programação, estrutura de dados e desenvolvimento Web e Mobile.',
        tags: ['ADS', 'Programação', 'Web', 'Mobile', 'Cloud'],
    },
]
</script>

<style scoped>
.about-section {
    padding: 120px 48px;
    max-width: 1100px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 80px;
    align-items: start;
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
    margin-bottom: 24px;
}

.about-text {
    color: var(--muted);
    line-height: 1.9;
}

.about-text p+p {
    margin-top: 16px;
}

/* TIMELINE */
.timeline {
    display: flex;
    flex-direction: column;
    gap: 0;
}

.timeline-item {
    display: grid;
    grid-template-columns: 24px 1fr;
    gap: 20px;
}

.timeline-left {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.timeline-dot {
    width: 24px;
    height: 24px;
    border-radius: 50%;
    border: 1px solid var(--border);
    background: var(--surface);
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
    transition: border-color 0.3s;
}

.timeline-item.active .timeline-dot {
    border-color: var(--accent);
    box-shadow: 0 0 12px rgba(0, 212, 255, 0.2);
}

.dot-inner {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: var(--muted);
    transition: background 0.3s;
}

.timeline-item.active .dot-inner {
    background: var(--accent);
    animation: pulse 2s ease infinite;
}

.timeline-line {
    width: 1px;
    flex: 1;
    background: var(--border);
    margin: 4px 0;
    min-height: 24px;
}

.timeline-body {
    padding-bottom: 36px;
}

.timeline-item:last-child .timeline-body {
    padding-bottom: 0;
}

.timeline-meta {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 8px;
}

.timeline-year {
    color: var(--accent);
}

.timeline-badge {
    color: var(--bg);
    background: var(--accent);
    padding: 2px 8px;
    font-size: 10px;
    letter-spacing: 0.1em;
    text-transform: uppercase;
}

.timeline-title {
    font-family: var(--sans);
    font-size: 15px;
    font-weight: 600;
    color: var(--text);
    margin-bottom: 8px;
    line-height: 1.3;
}

.timeline-desc {
    color: var(--muted);
    line-height: 1.8;
    margin-bottom: 12px;
}

.timeline-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
}

.tl-tag {
    color: var(--accent);
    border: 1px solid rgba(0, 212, 255, 0.15);
    padding: 3px 8px;
    letter-spacing: 0.06em;
    text-transform: uppercase;
}

@media (max-width: 768px) {
    .about-section {
        grid-template-columns: 1fr;
        gap: 48px;
        padding: 80px 24px;
    }
}
</style>