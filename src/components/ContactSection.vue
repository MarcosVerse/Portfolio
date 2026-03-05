<template>
    <section id="contact" class="contact-section">
        <div class="contact-inner">
            <div class="contact-left reveal">
                <div class="section-label font-mono text-xs uppercase tracking-widest">// contato</div>
                <div class="contact-big">
                    Vamos<br><span class="accent">construir</span><br>juntos?
                </div>
                <p class="contact-sub font-mono text-sm">
                    Aberto a projetos freelance, posições full-time e conversas sobre tecnologia. Respondo rápido.
                </p>
            </div>
            <div class="contact-right reveal">
                <div class="contact-links">
                    <a
                    v-for="link in links"
                    :key="link.label"
                    :href="link.href"
                    class="contact-link"
                    target="_blank"
                    >
                    <span class="link-icon" v-html="link.icon"></span>
                    <span class="link-label font-mono text-xs uppercase tracking-widest">{{ link.label }}</span>
                    <span class="link-val font-mono text-sm">{{ link.value }}</span>
                    <span class="link-arrow">↗</span>
                    </a>
                </div>
            </div>
        </div>
    </section>
</template>

<script setup>
import { useScrollReveal } from '../composables/useScrollReveal.js'

useScrollReveal()

const links = [
    {
        label: 'Email',
        value: 'dev.salles7@gmail.com',
        href: 'mailto:dev.salles7@gmail.com',
        icon: `<svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><rect width="20" height="16" x="2" y="4" rx="2"/><path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"/></svg>`,
    },
    {
        label: 'GitHub',
        value: 'github.com/MarcosVerse',
        href: 'https://github.com/MarcosVerse',
        icon: `<svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M15 22v-4a4.8 4.8 0 0 0-1-3.5c3 0 6-2 6-5.5.08-1.25-.27-2.48-1-3.5.28-1.15.28-2.35 0-3.5 0 0-1 0-3 1.5-2.64-.5-5.36-.5-8 0C6 2 5 2 5 2c-.3 1.15-.3 2.35 0 3.5A5.403 5.403 0 0 0 4 9c0 3.5 3 5.5 6 5.5-.39.49-.68 1.05-.85 1.65-.17.6-.22 1.23-.15 1.85v4"/><path d="M9 18c-4.51 2-5-2-7-2"/></svg>`,
    },
    {
        label: 'LinkedIn',
        value: 'in/marcossalles0',
        href: 'https://linkedin.com/in/marcossalles0',
        icon: `<svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"/><rect width="4" height="12" x="2" y="9"/><circle cx="4" cy="4" r="2"/></svg>`,
    },
    {
        label: 'WhatsApp',
        value: '+55 (41) 98537-8351',
        href: 'https://wa.me/5541985378351',
        icon: `<svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M21 11.5a8.38 8.38 0 0 1-.9 3.8 8.5 8.5 0 0 1-7.6 4.7 8.38 8.38 0 0 1-3.8-.9L3 21l1.9-5.7a8.38 8.38 0 0 1-.9-3.8 8.5 8.5 0 0 1 4.7-7.6 8.38 8.38 0 0 1 3.8-.9h.5a8.48 8.48 0 0 1 8 8v.5z"/></svg>`,
    },
]
</script>

<style scoped>
.contact-section {
    padding: 120px 48px;
    background: var(--surface);
    border-top: 1px solid var(--border);
}

.contact-inner {
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

.contact-big {
    font-family: var(--sans);
    font-size: clamp(40px, 5vw, 64px);
    font-weight: 800;
    letter-spacing: -0.03em;
    line-height: 1;
}

.contact-big .accent {
    color: var(--accent);
}

.contact-sub {
    color: var(--muted);
    line-height: 1.8;
    margin-top: 20px;
}

.contact-links {
    display: flex;
    flex-direction: column;
    gap: 1px;
    border: 1px solid var(--border);
    overflow: hidden;
}

.contact-link {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px 24px;
    border-bottom: 1px solid var(--border);
    text-decoration: none;
    color: var(--text);
    transition: background 0.2s;
    gap: 16px;
    cursor: none;
}

.contact-link:last-child {
    border-bottom: none;
}

.contact-link:hover {
    background: rgba(255, 255, 255, 0.03);
}

.link-label {
    color: var(--muted);
    min-width: 80px;
}

.link-val {
    color: var(--text);
    flex: 1;
}

.link-arrow {
    font-size: 14px;
    color: var(--accent);
    transition: transform 0.2s;
}

.link-icon {
    color: var(--accent);
    display: flex;
    align-items: center;
    flex-shrink: 0;
}

.contact-link:hover .link-arrow {
    transform: translate(4px, -4px);
}

@media (max-width: 768px) {
    .contact-section {
        padding: 80px 24px;
    }

    .contact-inner {
        grid-template-columns: 1fr;
        gap: 48px;
    }
}
</style>