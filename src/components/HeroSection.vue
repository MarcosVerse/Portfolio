<template>
    <section id="hero" class="hero">
        <div class="hero-glow"></div>
        <div class="hero-inner">
            <div class="hero-tag font-mono text-xs uppercase tracking-widest">
                Full Stack Developer
            </div>
            <h1>
                <span class="line">Marcos</span>
                <span class="line dim">Salles</span>
            </h1>
            <p class="hero-desc font-mono text-sm">
                    ALGUMA DESCRICAO ALGUMA DESCRICAO ALGUMA DESCRICAO ALGUMA DESCRICAO
                    ALGUMA DESCRICAO ALGUMA DESCRICAO ALGUMA DESCRICAO ALGUMA DESCRICAO
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
        <div class="hero-scroll-hint font-mono text-xs tracking-widest">
            <div class="scroll-line"></div>
            scroll
        </div>
    </section>
</template>

<script setup>
</script>

<style scoped>
.hero {
    min-height: 100vh;
    display: flex;
    align-items: center;
    padding: 0 48px;
    padding-top: 80px;
    position: relative;
}

.hero-glow {
    position: absolute;
    top: 10%;
    right: -10%;
    width: 600px;
    height: 600px;
    background: radial-gradient(circle, rgba(0, 212, 255, 0.06) 0%, transparent 65%);
    pointer-events: none;
}

.hero-inner {
    max-width: 900px;
    position: relative;
    z-index: 1;
}

.hero-tag {
    color: var(--accent);
    margin-bottom: 28px;
    display: flex;
    align-items: center;
    gap: 10px;
    animation: fadeInUp 0.6s ease 0.2s both;
}

.hero-tag::before {
    content: '';
    display: inline-block;
    width: 20px;
    height: 1px;
    background: var(--accent);
}

h1 {
    font-family: var(--sans);
    font-size: clamp(52px, 8vw, 100px);
    font-weight: 800;
    line-height: 0.95;
    letter-spacing: -0.03em;
    margin-bottom: 24px;
}

h1 .line {
    display: block;
    animation: fadeInUp 0.6s ease 0.3s both;
}

h1 .line.dim {
    color: var(--muted);
    animation-delay: 0.4s;
}

.hero-desc {
    color: var(--muted);
    line-height: 1.8;
    max-width: 480px;
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
    border: none;
    padding: 14px 28px;
    text-decoration: none;
    transition: all 0.2s;
    position: relative;
    overflow: hidden;
}

.btn-primary::after {
    content: '';
    position: absolute;
    inset: 0;
    background: white;
    transform: translateX(-101%);
    transition: transform 0.3s ease;
}

.btn-primary:hover::after {
    transform: translateX(0);
}

.btn-primary span {
    position: relative;
    z-index: 1;
}

.btn-ghost {
    color: var(--muted);
    text-decoration: none;
    padding: 14px 0;
    border-bottom: 1px solid var(--border);
    transition: all 0.2s;
    display: flex;
    align-items: center;
    gap: 8px;
}

.btn-ghost:hover {
    color: var(--text);
    border-color: var(--text);
}

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
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: var(--accent);
    animation: scrollLine 2s ease infinite;
}

@media (max-width: 768px) {
    .hero {
        padding: 80px 24px;
    }
}
</style>