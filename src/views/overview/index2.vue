<template>
    <div ref="appRef2" class="page"></div>
</template>

<script setup>
import { onMounted } from 'vue'


const scale = reactive({
    width: '1',
    height: '1',
})

// * 设计稿尺寸（px）
const baseWidth = 1920
const baseHeight = 1080

// * 需保持的比例（默认1.77778）
const baseProportion = parseFloat((baseWidth / baseHeight).toFixed(5))
const appRef2 = ref('appRef2')
function calcRate() {
    debugger
    const appRef = appRef2.value
    if (!appRef) return
    // 当前宽高比
    const currentRate = parseFloat((window.innerWidth / window.innerHeight).toFixed(5))
    console.log(window.innerWidth, window.innerHeight, currentRate)
    if (appRef) {
        if (currentRate > baseProportion) {
            // 表示更宽
            scale.width = ((window.innerHeight * baseProportion) / baseWidth).toFixed(5)
            scale.height = (window.innerHeight / baseHeight).toFixed(5)
            appRef.style.transform = `scale(${scale.width}, ${scale.height}) translate(-50%, -50%)`
        } else {
            // 表示更高
            scale.height = ((window.innerWidth / baseProportion) / baseHeight).toFixed(5)
            scale.width = (window.innerWidth / baseWidth).toFixed(5)
            appRef.style.transform = `scale(${scale.width}, ${scale.height}) translate(-50%, -50%)`
        }
        console.log(scale.width, scale.height)
    }
}

const drawTiming = ref(null)
function resize() {
    clearTimeout(drawTiming.value)
    drawTiming.value = setTimeout(() => {
        calcRate()
    }, 200)
}

onMounted(() => {
    calcRate()
    window.addEventListener('resize', resize)
})
</script>

<style lang="scss" scoped>
.page {
    width: 1920px;
    height: 1080px;
    background: url('./bj.awebp') no-repeat;
    background-size: 100% 100%;
    position: absolute;
    transform-origin: 0 0;
    left: 50%;
    top: 50%;
}
</style>