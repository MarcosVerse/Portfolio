<template>
    <div class="tech-item">
        <span class="text-2xl leading-none">{{ tech.icon }}</span>
        <span class="font-mono text-xs tracking-wide" style="color: var(--text)">{{ tech.name }}</span>
        <span class="font-mono text-[10px] uppercase tracking-widest" style="color: var(--muted)">{{ tech.category
            }}</span>
    </div>
</template>

<script setup>
defineProps({
    tech: {
        type: Object,
        required: true,
    },
})
</script>

<style scoped>
.tech-item {
    padding: 24px 20px;
    border-right: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
    display: flex;
    flex-direction: column;
    gap: 10px;
    transition: background 0.2s;
    position: relative;
    overflow: hidden;
    cursor: default;
}

.tech-item::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(0, 212, 255, 0.04), transparent);
    opacity: 0;
    transition: opacity 0.3s;
}

.tech-item:hover {
    background: rgba(255, 255, 255, 0.02);
}

.tech-item:hover::before {
    opacity: 1;
}
</style>