<template>
    <div class="robot-wrap" ref="robotWrap">
        <svg class="robot-svg" viewBox="0 0 60 90" width="60" height="90" overflow="visible">

            <!-- Antena -->
            <line ref="antenna" x1="30" y1="6" x2="30" y2="0" stroke="#f59e0b" stroke-width="1.5"/>
            <circle ref="antennaDot" cx="30" cy="0" r="2" fill="#f59e0b"/>

            <!-- Cabeça -->
            <rect ref="head" x="14" y="6" width="32" height="24" rx="4"
                fill="#0d1008" stroke="#f59e0b" stroke-width="1.2"/>
            <!-- Olhos -->
            <rect ref="eyeL" x="19" y="13" width="8" height="7" rx="1" fill="#f59e0b"/>
            <rect ref="eyeR" x="33" y="13" width="8" height="7" rx="1" fill="#f59e0b"/>
            <!-- Boca -->
            <rect x="22" y="24" width="16" height="2" rx="1" fill="#f59e0b" opacity="0.4"/>

            <!-- Pescoço -->
            <rect x="27" y="30" width="6" height="4" fill="#f59e0b" opacity="0.3"/>

            <!-- Corpo -->
            <rect ref="body" x="10" y="34" width="40" height="28" rx="3"
                fill="#0d1008" stroke="#f59e0b" stroke-width="1.2"/>
            <!-- Detalhe peito -->
            <rect x="22" y="40" width="16" height="10" rx="2"
                fill="#f59e0b" opacity="0.08" stroke="#f59e0b" stroke-width="0.8"/>
            <circle cx="30" cy="45" r="3" fill="#f59e0b" opacity="0.6"/>

            <!-- Braço esquerdo (grupo rotaciona do ombro) -->
            <g ref="armL" style="transform-origin: 10px 38px">
                <rect x="2" y="36" width="8" height="20" rx="3"
                    fill="#0d1008" stroke="#f59e0b" stroke-width="1"/>
                <!-- Mão esq -->
                <rect ref="handL" x="1" y="54" width="10" height="8" rx="2"
                    fill="#0d1008" stroke="#f59e0b" stroke-width="1"/>
            </g>

            <!-- Braço direito (grupo rotaciona do ombro) -->
            <g ref="armR" style="transform-origin: 50px 38px">
                <rect x="50" y="36" width="8" height="20" rx="3"
                    fill="#0d1008" stroke="#f59e0b" stroke-width="1"/>
                <!-- Mão dir -->
                <rect ref="handR" x="49" y="54" width="10" height="8" rx="2"
                    fill="#0d1008" stroke="#f59e0b" stroke-width="1"/>
            </g>

            <!-- Perna esquerda -->
            <g ref="legL" style="transform-origin: 20px 62px">
                <rect x="14" y="62" width="10" height="20" rx="3"
                    fill="#0d1008" stroke="#f59e0b" stroke-width="1"/>
                <!-- Pé -->
                <rect x="11" y="80" width="14" height="5" rx="2"
                    fill="#0d1008" stroke="#f59e0b" stroke-width="1"/>
            </g>

            <!-- Perna direita -->
            <g ref="legR" style="transform-origin: 40px 62px">
                <rect x="36" y="62" width="10" height="20" rx="3"
                    fill="#0d1008" stroke="#f59e0b" stroke-width="1"/>
                <!-- Pé -->
                <rect x="35" y="80" width="14" height="5" rx="2"
                    fill="#0d1008" stroke="#f59e0b" stroke-width="1"/>
            </g>

            <!-- Ícone carregando (aparece na mão) -->
            <g ref="carryGroup" opacity="0">
                <rect x="20" y="-20" width="20" height="20" rx="2"
                    fill="#0d1008" stroke="#f59e0b" stroke-width="1"/>
                <foreignObject x="22" y="-18" width="16" height="16">
                    <i xmlns="http://www.w3.org/1999/xhtml" :class="carryingIcon"
                        style="font-size:14px; display:block; text-align:center; line-height:16px;"></i>
                </foreignObject>
            </g>

        </svg>
    </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { gsap } from 'gsap'

const props = defineProps({
    factory: Object,
    techIcons: Array,
})

const robotWrap  = ref(null)
const head       = ref(null)
const eyeL       = ref(null)
const eyeR       = ref(null)
const armL       = ref(null)
const armR       = ref(null)
const legL       = ref(null)
const legR       = ref(null)
const carryGroup = ref(null)
const antennaDot = ref(null)
const carryingIcon = ref('')

let walkLoop = null
let blinkLoop = null
let stopped = false

function delay(ms) { return new Promise(r => setTimeout(r, ms)) }

// Posição atual
let curX = -80
let curY = window.innerHeight * 0.75

function setPos(x, y) {
    curX = x; curY = y
    gsap.set(robotWrap.value, { x, y })
}

function moveTo(x, y, dur = 1.2, ease = 'power2.inOut') {
    return new Promise(resolve => {
        // Vira na direção certa
        const dir = x > curX ? 1 : -1
        gsap.set(robotWrap.value, { scaleX: dir })
        gsap.to(robotWrap.value, {
            x, y, duration: dur, ease,
            onComplete: () => { curX = x; curY = y; resolve() }
        })
    })
}

function moveArc(tx, ty, dur = 1.0) {
    return new Promise(resolve => {
        const sx  = curX, sy = curY
        const mx  = (sx + tx) / 2
        const my  = Math.min(sy, ty) - 100
        const dir = tx > sx ? 1 : -1
        gsap.set(robotWrap.value, { scaleX: dir })

        const proxy = { t: 0 }
        gsap.to(proxy, {
            t: 1, duration: dur, ease: 'power2.inOut',
            onUpdate() {
                const t = proxy.t, mt = 1 - t
                const x = mt*mt*sx + 2*mt*t*mx + t*t*tx
                const y = mt*mt*sy + 2*mt*t*my + t*t*ty
                gsap.set(robotWrap.value, { x, y })
                curX = x; curY = y
            },
            onComplete: resolve,
        })
    })
}

// Animação de andar — balança pernas e braços
function startWalk() {
    walkLoop = gsap.timeline({ repeat: -1 })
    walkLoop
        .to(legL.value,  { rotation: 20,  duration: 0.25, ease: 'sine.inOut' })
        .to(legR.value,  { rotation: -20, duration: 0.25, ease: 'sine.inOut' }, '<')
        .to(armL.value,  { rotation: -15, duration: 0.25, ease: 'sine.inOut' }, '<')
        .to(armR.value,  { rotation: 15,  duration: 0.25, ease: 'sine.inOut' }, '<')
        .to(legL.value,  { rotation: -20, duration: 0.25, ease: 'sine.inOut' })
        .to(legR.value,  { rotation: 20,  duration: 0.25, ease: 'sine.inOut' }, '<')
        .to(armL.value,  { rotation: 15,  duration: 0.25, ease: 'sine.inOut' }, '<')
        .to(armR.value,  { rotation: -15, duration: 0.25, ease: 'sine.inOut' }, '<')
}

function stopWalk() {
    walkLoop?.kill()
    gsap.to([legL.value, legR.value, armL.value, armR.value], {
        rotation: 0, duration: 0.2
    })
}

// Bobbing idle
function startIdle() {
    gsap.to(robotWrap.value, {
        y: curY - 6, duration: 0.8,
        yoyo: true, repeat: -1, ease: 'sine.inOut',
        id: 'idle-bob'
    })
}

function stopIdle() {
    gsap.killTweensOf(robotWrap.value, 'y')
}

// Piscar
function startBlink() {
    const doBlink = () => {
        gsap.to([eyeL.value, eyeR.value], {
            scaleY: 0.1, duration: 0.06, yoyo: true, repeat: 1,
            onComplete: () => {
                if (!stopped) blinkLoop = setTimeout(doBlink, 2000 + Math.random() * 2000)
            }
        })
    }
    blinkLoop = setTimeout(doBlink, 1000)
}

// Animação de pegar — braço desce
function animGrab() {
    return new Promise(resolve => {
        gsap.timeline({ onComplete: resolve })
            .to(armR.value, { rotation: 60, duration: 0.3, ease: 'power2.out' })
            .to(armR.value, { rotation: 30, duration: 0.2, ease: 'power2.in' })
    })
}

// Animação de soltar — braço sobe
function animPlace() {
    return new Promise(resolve => {
        gsap.timeline({ onComplete: resolve })
            .to(armR.value, { rotation: -40, duration: 0.3, ease: 'power2.out' })
            .to(armR.value, { rotation: 0,   duration: 0.2, ease: 'bounce.out' })
    })
}

// Celebração
function celebrate() {
    return new Promise(resolve => {
        gsap.timeline({ onComplete: resolve })
            .to(armL.value, { rotation: -160, duration: 0.3 })
            .to(armR.value, { rotation: 160,  duration: 0.3 }, '<')
            .to(robotWrap.value, { y: curY - 20, duration: 0.2, yoyo: true, repeat: 3 })
            .to([armL.value, armR.value], { rotation: 0, duration: 0.3 })
    })
}

async function runCycle() {
    if (stopped) return

    const factory   = props.factory
    const slotDefs  = factory?.slotDefs
    const factoryEl = factory?.factoryEl?.value
    if (!slotDefs || !factoryEl) {
        await delay(300)
        return runCycle()
    }

    // Reset
    factory.resetSlots()
    await delay(300)

    // Entra pela esquerda andando
    setPos(-80, window.innerHeight * 0.75)
    startWalk()
    startBlink()
    await moveTo(window.innerWidth * 0.3, window.innerHeight * 0.75, 1.4)
    stopWalk()

    // Para cada slot
    for (let i = 0; i < slotDefs.length; i++) {
        const slotDef = slotDefs[i]
        const pool    = (props.techIcons || []).filter(t => t.type === slotDef.type)
        const tech    = pool[Math.floor(Math.random() * pool.length)]
        if (!tech) continue

        // Vai até a "prateleira" (parte inferior)
        const srcX = 80 + i * 90
        const srcY = window.innerHeight - 80
        startWalk()
        await moveTo(srcX, srcY, 0.8)
        stopWalk()

        // Pega o ícone
        await animGrab()
        carryingIcon.value = tech.icon
        gsap.to(carryGroup.value, { opacity: 1, duration: 0.2 })
        await delay(200)

        // Voa até o slot
        const rect  = factoryEl.getBoundingClientRect()
        const destX = rect.left + (slotDef.x / 100) * rect.width  - 30
        const destY = rect.top  + (slotDef.y / 100) * rect.height - 45
        await moveArc(destX, destY, 0.9)

        // Deposita
        await animPlace()
        gsap.to(carryGroup.value, { opacity: 0, duration: 0.15 })
        carryingIcon.value = ''
        factory.fillSlot(slotDef.id, tech)
        await delay(250)
    }

    // Processing
    factory.setProcessing()
    stopIdle()

    // Vai pro centro da factory comemorar
    const rect = factoryEl.getBoundingClientRect()
    startWalk()
    await moveTo(rect.left + rect.width / 2 - 30, rect.top + rect.height / 2 - 45, 0.8)
    stopWalk()
    startIdle()
    await celebrate()
    await delay(2000)
    stopIdle()

    // Sai pela direita
    startWalk()
    gsap.set(robotWrap.value, { scaleX: 1 })
    await moveTo(window.innerWidth + 80, window.innerHeight * 0.75, 1.2)
    stopWalk()

    await delay(500)
    runCycle()
}

onMounted(() => {
    setPos(-80, window.innerHeight * 0.75)
    setTimeout(runCycle, 800)
})

onUnmounted(() => {
    stopped = true
    walkLoop?.kill()
    clearTimeout(blinkLoop)
    gsap.killTweensOf(robotWrap.value)
})
</script>

<style scoped>
.robot-wrap {
    position: fixed;
    z-index: 200;
    pointer-events: none;
    /* gsap controla x/y */
}
</style>