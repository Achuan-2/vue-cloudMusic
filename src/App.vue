<template>
    <div id="app">
        <!-- 顶部 -->
        <top />
        <index />
        <vditor/>
    </div>
</template>

<script>
import top from './components/01.top.vue'
import index from './components/02.index.vue'
import vditor from './components/vditor.vue'
export default {
    name: 'app',
    components: {
        top,
        index,
        vditor
    },
}
</script>

<style >
</style>
