
<script setup>
import { reactive, watchEffect, watch, ref, onMounted, onBeforeUnmount } from 'vue'
import ConfigurationRow from './ConfigurationRow.vue';
import { isValidColor, colorLuminance, getSizes, getContrast, setCss } from '../utils/helper'
import LightSourceSwitcher from './LightSourceSwitcher.vue';
import ShapeSwitcher from './ShapeSwitcher.vue';
import CodePanel from './CodePanel.vue';
const { size, maxSize } = getSizes()
const data = reactive({
  size, /** 150-300 */
  maxSize, /** 180-400 */
  color: isValidColor(window.location.hash) ? window.location.hash : '#e0e0e0',
  colorDifference: 0.15, /** 颜色对比度 0.01-0.6 */
  radius: Math.round(size * .1), /**  */
  maxRadius: Math.round(size / 2), /** size/2 */
  distance: 30, /** 5-50*/
  blur: Math.round(size * .2), /** 阴影 模糊半径 */
  lightSource: 1, /** 1左上 2右上 3左下 4右下 */
  shape: 1,
  offsetX: Math.round(size * .1),  /** 阴影 X轴偏移量 */
  offsetY: Math.round(size * .1), /** 阴影 Y轴偏移量 */
  angle: 135, /** 渐变角度 */
  gradient: false, /** 是否渐变 */
  darkColor: '',
  lightColor: '',
  firstGradientColor: '',
  secondGradientColor: '',
  background: ' ',
  boxShadowPosition: '', /** box-shadow inset关键字 */
  boxShadow: '',
})
/**  set valid color */
const setValidColor = (value) => isValidColor(value) ? data.color = value : null
/** 处理color */
watchEffect(() => {
  window.history.replaceState('', '', '/' + data.color)
  console.log('color', data.color, data.colorDifference);
  data.darkColor = colorLuminance(data.color, data.colorDifference * -1)
  data.lightColor = colorLuminance(data.color, data.colorDifference)
  setCss(document.documentElement, {
    '--text-color': getContrast(data.color), /** 如果背景颜色偏白文字颜色要偏黑 反之 */
    '--base-color': data.color,
    '--dark-color': data.darkColor,
    '--light-color': data.lightColor
  })
})
/** 处理size */
watchEffect(() => {
  document.documentElement.style.setProperty('--size', data.size + 'px')
  data.distance = Math.round(data.size * .1) /** distance改变会触发blur改变 blur=distance*2 */
  data.maxRadius = Math.round(data.size / 2)
  console.log('size', data.size)
  // data.radius > data.maxRadius ? data.radius = data.maxRadius : null
})

/** 处理radius */
watch(() => data.radius, () => {
  document.documentElement.style.setProperty('--radius',
    data.radius === data.maxRadius ? '50%' : data.radius + 'px')
  console.log('radius:', data.radius)
})
/** 阴影偏移 渐变角度 */
const updateOffset = (distance, lightSource) => {
  data.offsetX = (lightSource % 2 === 1 ? distance : distance * -1)
  data.offsetY = (lightSource < 3 ? distance : distance * -1)
  data.angle = lightSource === 4 ? 45 : 45 + 90 * lightSource
  console.log('offset-x', data.offsetX, 'offset-y', data.offsetY, 'angle', data.angle)
  // setCss(document.documentElement, {
  //   '--positionX': data.offsetX + 'px',
  //   '--positionY': data.offsetY + 'px',
  //   '--angle': data.angle
  // })
}
/** 处理distance */
watch(() => data.distance, (distance) => {
  data.blur = distance * 2
  updateOffset(distance, data.lightSource)
  console.log('distance', distance);
})
/** blur */
watchEffect(() => {
  // document.documentElement.style.setProperty('--blur', data.blur + 'px')
  console.log('blur', data.blur)
})

/** 处理light source */
const setLightSource = (lightSource) => {
  data.lightSource = lightSource
  updateOffset(data.distance, data.lightSource)
  console.log('setLightSource', lightSource)
}
/** 处理shape */
const setShape = (shape) => {
  data.shape = shape
  data.gradient = data.shape === 2 || data.shape === 3
  data.boxShadowPosition = shape === 4 ? 'inset' : ''
  if (data.gradient) {
    data.firstGradientColor = colorLuminance(data.color, data.shape === 3 ? 0.07 : -0.1)
    data.secondGradientColor = colorLuminance(data.color, data.shape === 2 ? 0.07 : -0.1)
  }
  console.log('setShape', shape)
}
/** box-shadow */
watchEffect(() => {
  const { offsetX, offsetY, blur, boxShadowPosition, darkColor, lightColor } = data
  data.boxShadow = `${boxShadowPosition} ${offsetX}px ${offsetY}px ${blur}px ${darkColor},
    ${boxShadowPosition} ${offsetX * -1}px ${offsetY * -1}px ${blur}px ${lightColor}`
  document.documentElement.style.setProperty('--box-shadow', data.boxShadow)
  console.log('box-shadow', data.boxShadow)
})
/** background */
watchEffect(() => {
  data.background = data.gradient ? `linear-gradient(${data.angle}deg,${data.firstGradientColor},${data.secondGradientColor})` : data.color
  document.documentElement.style.setProperty('--background', data.background)
  console.log('background', data.background)
})
const setHashColor = () => {
  if (isValidColor(window.location.hash)) {
    data.color = window.location.hash
  } else {
    window.history.replaceState('', '', '/' + data.color)
  }
}
onMounted(() => {
  window.addEventListener('hashchange', setHashColor)
  getStyle()
})
onBeforeUnmount(() => {
  window.removeEventListener('hashchange', setHashColor)
})
const cssStyle = ref('')
watch(data, () => getStyle())
const getStyle = () => {
  cssStyle.value = `border-radius: ${data.radius}px;
background: ${data.background};
box-shadow: ${data.boxShadow};`
}

</script>
<template>
  <div class="configuration soft-shadow">
    <!-- color -->
    <div class="row">
      <label for="color">pick a color：</label>
      <input type="color" id="color" v-model="data.color">
      <label for="colorValue">or</label>
      <input type="text" placeholder="#ffffff" id="colorValue" :value="data.color"
        @input="setValidColor($event.target.value)">
    </div>
    <!-- size -->
    <ConfigurationRow v-model="data.size" :label="'Size'" :min="10" :max="data.maxSize" />
    <!-- radius -->
    <ConfigurationRow v-model="data.radius" :label="'Radius'" :min="10" :max="data.maxRadius" />
    <!-- distance -->
    <ConfigurationRow v-model="data.distance" :label="'Distance'" :min="5" :max="50" />
    <!-- colorDifference -->
    <ConfigurationRow v-model="data.colorDifference" :label="'Intensity'" :min="0.01" :max="0.6" :step="0.01" />
    <!-- blur -->
    <ConfigurationRow v-model="data.blur" :label="'Blur'" :min="0" :max="100" />
    <!-- shape -->
    <ShapeSwitcher :shape="data.shape" @update:shape="setShape"></ShapeSwitcher>
    <!-- light source -->
    <LightSourceSwitcher :lightSource="data.lightSource" :setLightSource="setLightSource"></LightSourceSwitcher>
    <!-- code panel -->
    <CodePanel :codeString="cssStyle"></CodePanel>
  </div>
</template>
<style scoped  lang="less">
.configuration {
  display: flex;
  flex-direction: column;
  padding: 20px;
  border-radius: 30px;
  position: relative;
}

input[type=text],
input[type=color] {
  width: 32px;
  border: 3px solid var(--text-color);
  height: 32px;
}

input[type=text] {
  padding: 0 5px;
  font-size: 15px;
  font-weight: 700;
  width: 85px;
}

input[type=color] {
  padding: 0;
  cursor: pointer;
}

/** input color 铺满 */
input[type=color]::-webkit-color-swatch-wrapper {
  padding: 0;
}

input[type=color]::-webkit-color-swatch {
  border: none;
}
</style>