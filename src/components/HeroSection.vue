<template>
    <section id="hero" class="hero" ref="heroRef">
        <div class="hero-glow" ref="glowRef"></div>

        <div class="hero-inner">
            <div class="hero-tag font-mono text-xs uppercase tracking-widest">
                <span>{{ displayedTag }}</span><span class="cursor-blink">|</span>
            </div>
            <h1>
                <span class="line glitch" data-text="Marcos">Marcos</span>
                <span class="line dim">Salles</span>
            </h1>
            <p class="hero-desc font-mono text-sm">
                Construo produtos digitais completos — do banco de dados à interface.
                Apaixonado por código limpo, experiências fluidas e soluções que fazem sentido.
            </p>
            <div class="hero-actions">
                <a href="#projects" class="btn-primary font-mono text-xs uppercase tracking-widest">
                    <span>Ver projetos</span>
                </a>
                <a href="#contact" class="btn-ghost font-mono text-xs uppercase tracking-widest">
                    Falar comigo →
                </a>
            </div>
        </div>

        <AppFactory />

        <div class="hero-scroll-hint font-mono text-xs tracking-widest">
            <div class="scroll-line"></div>
            scroll
        </div>
    </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import AppFactory from './AppFactory.vue'

const heroRef = ref(null)
const glowRef = ref(null)
const displayedTag = ref('')
const fullText = 'Full Stack Developer'

onMounted(() => {
    let i = 0
    const type = () => {
        if (i < fullText.length) {
            displayedTag.value += fullText[i++]
            setTimeout(type, 60)
        }
    }
    setTimeout(type, 400)

    heroRef.value.addEventListener('mousemove', (e) => {
        const x = (e.clientX / window.innerWidth - 0.5) * 40
        const y = (e.clientY / window.innerHeight - 0.5) * 40
        glowRef.value.style.transform = `translate(${x}px, ${y}px)`
    })
})
</script>

<style scoped>
.hero {
    min-height: 100vh;
    display: flex;
    align-items: center;
    padding: 0 0 0 50px;
    padding-top: 80px;
    position: relative;
    overflow: hidden;
    gap: 350px;
}

.hero-glow {
    position: absolute;
    top: 10%;
    right: -10%;
    width: 700px;
    height: 700px;
    background: radial-gradient(circle, rgba(245, 158, 11, 0.07) 0%, transparent 65%);
    pointer-events: none;
    transition: transform 0.15s ease;
}

.hero-inner {
    max-width: 480px;
    position: relative;
    z-index: 1;
    flex-shrink: 0;
}

.hero-tag {
    color: var(--accent);
    margin-bottom: 28px;
    display: flex;
    align-items: center;
    gap: 10px;
    animation: fadeInUp 0.6s ease 0.2s both;
    min-height: 20px;
}

.hero-tag::before {
    content: '';
    display: inline-block;
    width: 20px;
    height: 1px;
    background: var(--accent);
    flex-shrink: 0;
}

.cursor-blink {
    animation: blink 1s step-end infinite;
    color: var(--accent);
}

@keyframes blink {
    0%, 100% { opacity: 1; }
    50%       { opacity: 0; }
}

h1 {
    font-family: var(--sans);
    font-size: clamp(48px, 6vw, 96px);
    font-weight: 800;
    line-height: 0.95;
    letter-spacing: -0.03em;
    margin-bottom: 24px;
}

h1 .line { display: block; animation: fadeInUp 0.6s ease 0.3s both; }
h1 .line.dim { color: var(--muted); animation-delay: 0.4s; }

.glitch { position: relative; }
.glitch::before, .glitch::after {
    content: attr(data-text);
    position: absolute;
    top: 0; left: 0;
    width: 100%; height: 100%;
}
.glitch::before {
    color: var(--accent);
    animation: glitch-1 4s infinite;
    clip-path: polygon(0 0, 100% 0, 100% 40%, 0 40%);
}
.glitch::after {
    color: var(--accent2);
    animation: glitch-2 4s infinite;
    clip-path: polygon(0 60%, 100% 60%, 100% 100%, 0 100%);
}

@keyframes glitch-1 {
    0%, 90%, 100% { transform: translateX(0); opacity: 0; }
    92% { transform: translateX(-4px) skewX(-2deg); opacity: 0.7; }
    94% { transform: translateX(4px); opacity: 0.5; }
    96% { transform: translateX(-2px); opacity: 0.7; }
    98% { transform: translateX(0); opacity: 0; }
}
@keyframes glitch-2 {
    0%, 90%, 100% { transform: translateX(0); opacity: 0; }
    93% { transform: translateX(4px) skewX(2deg); opacity: 0.6; }
    95% { transform: translateX(-4px); opacity: 0.4; }
    97% { transform: translateX(2px); opacity: 0.6; }
    99% { transform: translateX(0); opacity: 0; }
}

.hero-desc {
    color: var(--muted);
    line-height: 1.8;
    max-width: 440px;
    margin-bottom: 48px;
    animation: fadeInUp 0.6s ease 0.5s both;
}

.hero-actions {
    display: flex;
    gap: 16px;
    align-items: center;
    animation: fadeInUp 0.6s ease 0.6s both;
}

.btn-primary {
    color: var(--bg);
    background: var(--accent);
    padding: 14px 28px;
    text-decoration: none;
    transition: all 0.2s;
    position: relative;
    overflow: hidden;
    cursor: none;
}
.btn-primary::after {
    content: '';
    position: absolute;
    inset: 0;
    background: white;
    transform: translateX(-101%);
    transition: transform 0.3s ease;
}
.btn-primary:hover::after { transform: translateX(0); }
.btn-primary span { position: relative; z-index: 1; }

.btn-ghost {
    color: var(--muted);
    text-decoration: none;
    padding: 14px 0;
    border-bottom: 1px solid var(--border);
    transition: all 0.2s;
    display: flex;
    align-items: center;
    gap: 8px;
    cursor: none;
}

.btn-ghost:hover { color: var(--text); border-color: var(--text); }

.hero-scroll-hint {
    position: absolute;
    bottom: 40px;
    left: 48px;
    color: var(--muted);
    display: flex;
    align-items: center;
    gap: 12px;
    animation: fadeInUp 0.6s ease 1s both;
    z-index: 1;
}

.scroll-line {
    width: 40px;
    height: 1px;
    background: var(--muted);
    position: relative;
    overflow: hidden;
}
.scroll-line::after {
    content: '';
    position: absolute;
    top: 0; left: -100%;
    width: 100%; height: 100%;
    background: var(--accent);
    animation: scrollLine 2s ease infinite;
}

@media (max-width: 1024px) {
    .hero { flex-direction: column; padding: 100px 24px 80px; }
    .hero-inner { max-width: 100%; }
}
</style>