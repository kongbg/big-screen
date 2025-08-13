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

const appRef2 = ref('appRef2')
function calcRate() {
    const appRef = appRef2.value
    if (!appRef) return
    // 当前宽高比
    if (appRef) {
        scale.width = (1 + ((window.innerWidth - baseWidth) / baseWidth)).toFixed(5)
        scale.height = (1 + ((window.innerHeight - baseHeight) / baseHeight)).toFixed(5)

        console.log(scale.width, scale.height)

        appRef.style.transform = `scale(${scale.width}, ${scale.height}) translate(-50%, -50%)`
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