<template>
    <div class="factory" ref="factoryRef">
        <div class="factory-label font-mono">
            <span class="accent">// </span>app.factory
        </div>

        <div class="factory-stage">
            <svg class="factory-svg" ref="svgRef" width="100%" height="100%"></svg>

            <div v-for="slot in slots" :key="slot.id" class="slot"
                :class="[`slot--${slot.state}`, `slot--${slot.type}`]"
                :style="{ left: slot.x + '%', top: slot.y + '%' }">
                <div class="slot-box">
                    <div class="slot-empty-inner" v-if="slot.state === 'empty'">
                        <div class="slot-plus">+</div>
                        <div class="slot-type-label font-mono">{{ slot.type }}</div>
                    </div>
                    <div class="slot-filled-inner" v-else-if="slot.state === 'filled'">
                        <i :class="slot.icon" class="slot-icon"></i>
                        <div class="slot-name font-mono">{{ slot.label }}</div>
                    </div>
                    <div class="slot-processing-inner" v-else-if="slot.state === 'processing'">
                        <div class="slot-spinner"></div>
                    </div>
                </div>
            </div>

            <div class="factory-output always-visible" :class="{ ready: outputVisible }">
                <div class="slot-box output-slot">
                    <div class="slot-empty-inner" v-if="!outputVisible">
                        <div class="slot-plus">+</div>
                        <div class="slot-type-label font-mono">output</div>
                    </div>
                    <div class="slot-filled-inner" v-else>
                        <div class="output-label font-mono">Full Stack<br>App</div>
                    </div>
                </div>
                <div class="output-particles" v-if="outputVisible">
                    <div v-for="n in 8" :key="n" class="output-particle" :style="particleStyle(n)"></div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue'

const factoryRef = ref(null)
const svgRef = ref(null)
const outputVisible = ref(false)

const slotDefs = [
    { id: 'frontend', type: 'frontend', x: 10, y: 28 },
    { id: 'language', type: 'language', x: 10, y: 72 },
    { id: 'api', type: 'api', x: 38, y: 50 },
    { id: 'database', type: 'database', x: 66, y: 28 },
    { id: 'infra', type: 'infra', x: 66, y: 72 },
]
const techPool = {
    frontend: [
        { icon: 'devicon-vuejs-plain colored', label: 'Vue.js' },
        { icon: 'devicon-react-original colored', label: 'React' },
    ],
    language: [
        { icon: 'devicon-typescript-plain colored', label: 'TypeScript' },
        { icon: 'devicon-javascript-plain colored', label: 'JavaScript' },
    ],
    api: [
        { icon: 'devicon-quarkus-plain colored', label: 'Quarkus' },
        { icon: 'devicon-java-plain colored', label: 'Java' },
    ],
    database: [
        { icon: 'devicon-postgresql-plain colored', label: 'PostgreSQL' },
        { icon: 'devicon-mysql-plain colored', label: 'MySQL' },
    ],
    infra: [
        { icon: 'devicon-docker-plain colored', label: 'Docker' },
        { icon: 'devicon-linux-plain', label: 'Linux' },
    ],
}

const slots = ref(slotDefs.map(d => ({ ...d, state: 'empty', icon: '', label: '' })))

const edges = [
    { from: 'frontend', to: 'api' },
    { from: 'language', to: 'api' },
    { from: 'api',      to: 'database' },
    { from: 'api',      to: 'infra' },
    { from: 'database', to: 'output' },
    { from: 'infra',    to: 'output' },
]
function drawEdges() {
    const svg = svgRef.value
    const el  = factoryRef.value
    if (!svg || !el) return

    const W = el.clientWidth
    const H = el.clientHeight
    svg.setAttribute('viewBox', `0 0 ${W} ${H}`)
    while (svg.firstChild) svg.removeChild(svg.firstChild)

    const allNodes = [
        ...slots.value,
        { id: 'output', x: 89, y: 50 }  
    ]
    const slotMap = Object.fromEntries(allNodes.map(s => [s.id, s]))

    edges.forEach(edge => {
        const a = slotMap[edge.from]
        const b = slotMap[edge.to]
        if (!a || !b) return

        const x1 = (a.x / 100) * W
        const y1 = (a.y / 100) * H
        const x2 = (b.x / 100) * W
        const y2 = (b.y / 100) * H

        const mx = (x1 + x2) / 2
        const path = document.createElementNS('http://www.w3.org/2000/svg', 'path')
        path.setAttribute('d', `M ${x1} ${y1} C ${mx} ${y1}, ${mx} ${y2}, ${x2} ${y2}`)
        path.setAttribute('stroke', 'rgba(245,158,11,0.2)')
        path.setAttribute('stroke-width', '1')
        path.setAttribute('fill', 'none')
        path.setAttribute('stroke-dasharray', '4 3')
        svg.appendChild(path)

        const angle = Math.atan2(y2 - y1, x2 - x1)
        const arrow = document.createElementNS('http://www.w3.org/2000/svg', 'polygon')
        const ax = x2 - Math.cos(angle) * 8
        const ay = y2 - Math.sin(angle) * 8
        arrow.setAttribute('points', `
            ${x2},${y2}
            ${ax + Math.sin(angle) * 4},${ay - Math.cos(angle) * 4}
            ${ax - Math.sin(angle) * 4},${ay + Math.cos(angle) * 4}
        `)
        arrow.setAttribute('fill', 'rgba(245,158,11,0.35)')
        svg.appendChild(arrow)
    })
}

function particleStyle(n) {
    const angle = (n / 8) * 360
    const dist = 40 + Math.random() * 20
    return {
        '--angle': angle + 'deg',
        '--dist': dist + 'px',
        animationDelay: (n * 0.08) + 's',
    }
}

let cycleTimer = null

async function runCycle() {
    outputVisible.value = false

    slots.value.forEach(s => { s.state = 'empty'; s.icon = ''; s.label = '' })
    await nextTick(); drawEdges()

    for (const slot of slots.value) {
        await delay(800)
        const pool = techPool[slot.type]
        const pick = pool[Math.floor(Math.random() * pool.length)]
        slot.icon = pick.icon
        slot.label = pick.label
        slot.state = 'filled'
        await nextTick(); drawEdges()
    }

    await delay(600)
    slots.value.forEach(s => s.state = 'processing')

    await delay(1400)

    slots.value.forEach(s => s.state = 'filled')
    outputVisible.value = true

    await delay(3000)

    runCycle()
}

function delay(ms) { return new Promise(r => setTimeout(r, ms)) }

onMounted(() => {
    nextTick(() => {
        drawEdges()
        setTimeout(runCycle, 600)
    })
    window.addEventListener('resize', drawEdges)
})

onUnmounted(() => {
    clearTimeout(cycleTimer)
    window.removeEventListener('resize', drawEdges)
})
</script>

<style scoped>
.factory {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 14px;
    min-width: 0;
    max-width: 640px;
    padding-right: 24px;
}

.factory-label {
    font-size: 10px;
    color: var(--muted);
    letter-spacing: 0.14em;
    text-transform: uppercase;
}

.factory-label .accent {
    color: var(--accent);
}

.factory-stage {
    position: relative;
    height: 340px;
    border: 1px solid var(--border);
    background: rgba(255, 255, 255, 0.01);
    overflow: visible;
}


.factory-svg {
    position: absolute;
    inset: 0;
    pointer-events: none;
    overflow: visible;
}

.slot {
    position: absolute;
    transform: translate(-50%, -50%);
}

.slot-box {
    width: 72px;
    height: 72px;
    border: 1px solid var(--border);
    background: var(--surface);
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    transition: border-color 0.3s, background 0.3s;
}

.slot--filled .slot-box {
    border-color: rgba(245, 158, 11, 0.4);
    background: var(--surface);;
}

.slot--processing .slot-box {
    border-color: rgba(245, 158, 11, 0.7);
    background: var(--surface);
    animation: slot-pulse 0.6s ease infinite alternate;
}

@keyframes slot-pulse {
    from {
        box-shadow: 0 0 0 rgba(245, 158, 11, 0);
    }

    to {
        box-shadow: 0 0 16px rgba(245, 158, 11, 0.3);
    }
}

.slot-empty-inner {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4px;
}

.slot-plus {
    font-size: 18px;
    color: var(--border);
    line-height: 1;
}

.slot-type-label {
    font-size: 7px;
    color: var(--muted);
    letter-spacing: 0.1em;
    text-transform: uppercase;
}

.slot-filled-inner {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 5px;
    animation: slot-fill 0.35s cubic-bezier(0.34, 1.56, 0.64, 1) both;
}

@keyframes slot-fill {
    from {
        opacity: 0;
        transform: scale(0.5) translateY(6px);
    }

    to {
        opacity: 1;
        transform: scale(1) translateY(0);
    }
}

.slot-icon {
    font-size: 28px;
}

.slot-name {
    font-size: 7px;
    color: var(--accent);
    letter-spacing: 0.08em;
    white-space: nowrap;
}

.slot-processing-inner {
    display: flex;
    align-items: center;
    justify-content: center;
}

.slot-spinner {
    width: 24px;
    height: 24px;
    border: 2px solid rgba(245, 158, 11, 0.2);
    border-top-color: var(--accent);
    border-radius: 50%;
    animation: spin 0.7s linear infinite;
}

@keyframes spin {
    to {
        transform: rotate(360deg);
    }
}

.factory-output {
    position: absolute;
    right: 26px;
    top: 50%;
    transform: translateY(-50%);
    pointer-events: none;
}

.output-slot {
    width: 72px;
    height: 72px;
    border: 1px solid var(--border);
    background: var(--surface);
    display: flex;
    align-items: center;
    justify-content: center;
    transition: border-color 0.5s, background 0.5s, box-shadow 0.5s;
}

.factory-output.ready .output-slot {
    border-color: rgba(245, 158, 11, 0.8);
    background: var(--surface);
    box-shadow: 0 0 24px rgba(245, 158, 11, 0.3), inset 0 0 12px rgba(245, 158, 11, 0.05);
    animation: ready-pulse 1.5s ease infinite alternate;
}


.output-box {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 6px;
    padding: 12px 16px;
    border: 1px solid rgba(245, 158, 11, 0.5);
    background: rgba(245, 158, 11, 0.08);
    white-space: nowrap;
    position: relative;
    z-index: 1;
}

.output-icon {
    font-size: 22px;
}

.output-label {
    font-size: 8px;
    color: var(--accent);
    letter-spacing: 0.08em;
    text-transform: uppercase;
    text-align: center;
    line-height: 1.6;
    animation: slot-fill 0.4s cubic-bezier(0.34,1.56,0.64,1) both;
}

.output-particles {
    position: absolute;
    inset: 0;
    pointer-events: none;
}

.output-particle {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 3px;
    height: 3px;
    background: var(--accent);
    border-radius: 50%;
    animation: particle-burst 0.8s ease-out both;
}

@keyframes ready-pulse {
    from { box-shadow: 0 0 12px rgba(245,158,11,0.2); }
    to   { box-shadow: 0 0 28px rgba(245,158,11,0.5); }
}


@keyframes particle-burst {
    from {
        transform: translate(-50%, -50%) rotate(var(--angle)) translateX(0);
        opacity: 1;
    }

    to {
        transform: translate(-50%, -50%) rotate(var(--angle)) translateX(var(--dist));
        opacity: 0;
    }
}

</style>