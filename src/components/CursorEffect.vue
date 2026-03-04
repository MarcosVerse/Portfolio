<template>
    <div class="cursor" ref="cursor"></div>
    <div class="cursor-ring" ref="ring"></div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const cursor = ref(null)
const ring = ref(null)

let mx = 0, my = 0, rx = 0, ry = 0
let animFrame

function onMouseMove(e) {
    mx = e.clientX
    my = e.clientY
    cursor.value.style.left = mx - 4 + 'px'
    cursor.value.style.top = my - 4 + 'px'
}

function animateRing() {
    rx += (mx - rx - 16) * 1
    ry += (my - ry - 16) * 1
    ring.value.style.left = rx + 'px'
    ring.value.style.top = ry + 'px'
    animFrame = requestAnimationFrame(animateRing)
}

function onEnter() {
    cursor.value.style.transform = 'scale(2.5)'
    ring.value.style.transform = 'scale(1.5)'
    ring.value.style.borderColor = 'rgba(0,212,255,0.8)'
}

function onLeave() {
    cursor.value.style.transform = 'scale(1)'
    ring.value.style.transform = 'scale(1)'
    ring.value.style.borderColor = 'rgba(0,212,255,0.4)'
}

onMounted(() => {
    document.addEventListener('mousemove', onMouseMove)
    document.querySelectorAll('a, button').forEach(el => {
        el.addEventListener('mouseenter', onEnter)
        el.addEventListener('mouseleave', onLeave)
    })
    animateRing()
})

onUnmounted(() => {
    document.removeEventListener('mousemove', onMouseMove)
    cancelAnimationFrame(animFrame)
})
</script>

<style scoped>
.cursor {
    width: 8px;
    height: 8px;
    background: var(--accent);
    border-radius: 50%;
    position: fixed;
    pointer-events: none;
    z-index: 9999;
    transition: transform 0.1s ease;
    mix-blend-mode: screen;
}

.cursor-ring {
    width: 32px;
    height: 32px;
    border: 1px solid rgba(0, 212, 255, 0.4);
    border-radius: 50%;
    position: fixed;
    pointer-events: none;
    z-index: 9998;
    transition: all 0.15s ease;
}
</style>