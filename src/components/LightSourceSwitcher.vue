<script setup>
import { ref, inject, onMounted } from 'vue';
import LightSource from './LightSource.vue';
defineProps(['lightSource', 'setLightSource'])
const lightSourceSwitcher = ref(null)
const preview = inject('preview')
onMounted(() => {
  // 将light-source-switch节点移动到preview节点
  preview.value.appendChild(lightSourceSwitcher.value)
  // preview.value.__vnode.el.appendChild(lightSourceSwitcher.value)
})
</script>
<template>
  <div class="light-source-switch" ref="lightSourceSwitcher">
    <LightSource :class="{ active: lightSource === 1 }" @click="setLightSource(1)" :left="0" :top="0" title="left top"
      data-value="1" :borderRadius="'0 0 30px 0'"></LightSource>
    <LightSource :class="{ active: lightSource === 2 }" @click="setLightSource(2)" :right="0" :top="0" data-value="2"
      :borderRadius="'0 0 0 30px'">
    </LightSource>
    <LightSource :class="{ active: lightSource === 3 }" @click="setLightSource(3)" :left="0" :bottom="0" data-value="3"
      :borderRadius="'0 30px 0 0'">
    </LightSource>
    <LightSource :class="{ active: lightSource === 4 }" @click="setLightSource(4)" :right="0" :bottom="0" data-value="4"
      :borderRadius="'30px 0 0 0'">
    </LightSource>
  </div>
</template> 