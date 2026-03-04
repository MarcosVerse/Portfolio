<template>
    <footer class="footer">
        <div class="footer-copy font-mono text-xs tracking-wider">
            © 2025 Marcos Salles
        </div>
        <div class="footer-status font-mono text-xs tracking-wider">
            <div class="status-dot"></div>
            disponível para projetos
        </div>
    </footer>
</template>

<script setup>
</script>

<style scoped>
.footer {
    padding: 32px 48px;
    border-top: 1px solid var(--border);
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: relative;
    z-index: 1;
}

.footer-copy {
    color: var(--muted);
}

.footer-status {
    display: flex;
    align-items: center;
    gap: 8px;
    color: var(--muted);
}

.status-dot {
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: var(--green);
    box-shadow: 0 0 8px var(--green);
    animation: pulse 2s ease infinite;
}

@media (max-width: 768px) {
    .footer {
        padding: 24px;
        flex-direction: column;
        gap: 12px;
    }
}
</style>