<template>
    <div class="cursor" ref="cursor"></div>
    <div class="cursor-ring" ref="ring"></div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const cursor = ref(null)
const ring   = ref(null)

let mx = 0, my = 0, rx = 0, ry = 0
let animFrame
let glitchInterval

const shapes = [
    '50%',
    '0%',
    '50% 0%',
    '0% 50%',
    '30% 70% 70% 30% / 30% 30% 70% 70%',
]

let shapeIndex = 0
let isGlitching = false
let isHovering = false

function glitch() {
    if (isGlitching) return
    isGlitching = true

    const r = ring.value
    if (!r) return

    let steps = 0
    const maxSteps = 4 + Math.floor(Math.random() * 3)

    const flicker = () => {
        if (steps >= maxSteps) {
            shapeIndex = (shapeIndex + 1) % shapes.length
            r.style.borderRadius = shapes[shapeIndex]
            r.style.opacity = '1'
            r.style.transform = `translate(-50%, -50%) scale(${isHovering ? 1.5 : 1})`
            isGlitching = false
            return
        }

        r.style.borderRadius = shapes[Math.floor(Math.random() * shapes.length)]
        r.style.opacity = Math.random() > 0.4 ? '1' : '0.3'
        r.style.transform = `translate(-50%, -50%) scale(${1 + Math.random() * 0.2})`

        steps++
        setTimeout(flicker, 40 + Math.random() * 40)
    }

    flicker()
}

function onMouseMove(e) {
    mx = e.clientX
    my = e.clientY
    cursor.value.style.left = mx - 2 + 'px'
    cursor.value.style.top  = my - 2 + 'px'
}

function animateRing() {
    rx += (mx - rx) * 0.18
    ry += (my - ry) * 0.18
    ring.value.style.left = rx + 'px'
    ring.value.style.top  = ry + 'px'
    animFrame = requestAnimationFrame(animateRing)
}

function onEnter() {
    isHovering = true
    cursor.value.style.transform = 'scale(2)'
    ring.value.style.borderColor = 'rgba(245,158,11,0.8)'
    glitch()
}

function onLeave() {
    isHovering = false
    cursor.value.style.transform = 'scale(1)'
    ring.value.style.transform = 'translate(-50%, -50%) scale(1)'
    ring.value.style.borderColor = 'rgba(245,158,11,0.4)'
}

function scheduleRandomGlitch() {
    const next = 2000 + Math.random() * 3000
    glitchInterval = setTimeout(() => {
        glitch()
        scheduleRandomGlitch()
    }, next)
}

onMounted(() => {
    ring.value.style.transform = 'translate(-50%, -50%) scale(1)'

    document.addEventListener('mousemove', onMouseMove)
    document.querySelectorAll('a, button').forEach(el => {
        el.addEventListener('mouseenter', onEnter)
        el.addEventListener('mouseleave', onLeave)
    })
    animateRing()
    scheduleRandomGlitch()
})

onUnmounted(() => {
    document.removeEventListener('mousemove', onMouseMove)
    cancelAnimationFrame(animFrame)
    clearTimeout(glitchInterval)
})
</script>

<style scoped>
.cursor {
    width: 4px;
    height: 4px;
    background: var(--accent);
    border-radius: 50%;
    position: fixed;
    pointer-events: none;
    z-index: 9999;
    transition: transform 0.1s ease;
    mix-blend-mode: screen;
}

.cursor-ring {
    width: 18px;
    height: 18px;
    border: 1px solid rgba(245,158,11,0.4);
    border-radius: 50%;
    position: fixed;
    pointer-events: none;
    z-index: 9998;
    transition: border-color 0.15s ease, opacity 0.05s ease;
}
</style>