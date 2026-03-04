<template>
    <div class="project-card">
        <span class="font-mono text-xs tracking-widest" style="color: var(--muted)">{{ project.num }}</span>
        <h3 class="text-xl font-bold tracking-tight" style="font-family: var(--sans)">{{ project.title }}</h3>
        <p class="font-mono text-xs leading-loose flex-1" style="color: var(--muted)">{{ project.description }}</p>
        <div class="flex flex-wrap gap-1.5">
            <span v-for="tag in project.tags" :key="tag"
                class="tag font-mono text-[10px] uppercase tracking-wider px-2.5 py-1">
                {{ tag }}
            </span>
        </div>
        <a :href="project.link" class="project-link font-mono text-xs uppercase tracking-widest">
            Ver projeto →
        </a>
    </div>
</template>

<script setup>
defineProps({
    project: {
        type: Object,
        required: true,
    },
})
</script>

<style scoped>
.project-card {
    padding: 40px;
    border-right: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
    display: flex;
    flex-direction: column;
    gap: 20px;
    position: relative;
    overflow: hidden;
    transition: background 0.3s;
}

.project-card::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 2px;
    height: 0;
    background: var(--accent);
    transition: height 0.4s ease;
}

.project-card:hover {
    background: rgba(255, 255, 255, 0.02);
}

.project-card:hover::after {
    height: 100%;
}

.tag {
    color: var(--accent);
    border: 1px solid rgba(0, 212, 255, 0.2);
}

.project-link {
    color: var(--muted);
    text-decoration: none;
    transition: color 0.2s;
}

.project-card:hover .project-link {
    color: var(--accent);
}
</style>