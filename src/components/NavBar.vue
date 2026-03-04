<template>
    <nav class="navbar">
        <div class="font-mono text-xs tracking-wider" style="color: var(--accent)">
            <span style="color: var(--muted)">~/</span>marcos-salles
        </div>
        <ul class="flex gap-8 list-none">
            <li v-for="link in links" :key="link.href">
                <a
                :href="link.href"
                class="font-mono text-xs uppercase tracking-widest transition-colors duration-200"
                style="color: var(--muted); text-decoration: none"
                @mouseenter="e => e.target.style.color = 'var(--text)'"
                @mouseleave="e => e.target.style.color = 'var(--muted)'"
                >
                {{ link.label }}
                </a>
            </li>
        </ul>
    </nav>
</template>

<script setup>
const links = [
    { href: '#about', label: 'Sobre' },
    { href: '#tech', label: 'Tech' },
    { href: '#projects', label: 'Projetos' },
    { href: '#contact', label: 'Contato' },
]
</script>

<style scoped>
.navbar {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    padding: 20px 48px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    z-index: 100;
    backdrop-filter: blur(12px);
    border-bottom: 1px solid var(--border);
    background: rgba(8, 12, 16, 0.7);
}

@media (max-width: 768px) {
    .navbar {
        padding: 16px 24px;
    }

    ul {
        display: none;
    }
}
</style>