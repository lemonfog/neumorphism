<script setup>
import IconFlat from './svg/IconFlat.vue';
import IconConcave from './svg/IconConcave.vue';
import IconConvex from './svg/IconConvex.vue';
import IconPressed from './svg/IconPressed.vue';
defineProps(['shape'])
defineEmits(['update:shape'])
</script>
<template>
  <div class="row shape-switch">
    <label>Shape:</label>
    <button :class="{ active: shape === 1 }" title="flat" data-shape="1" @click="$emit('update:shape', 1)">
      <IconFlat />
    </button>
    <button :class="{ active: shape === 2 }" title="concave" data-shape="2" @click="$emit('update:shape', 2)">
      <IconConcave />
    </button>
    <button :class="{ active: shape === 3 }" title="convex" data-shape="3" @click="$emit('update:shape', 3)">
      <IconConvex />
    </button>
    <button :class="{ active: shape === 4 }" title="pressed" data-shape="4" @click="$emit('update:shape', 4)">
      <IconPressed />
    </button>
  </div>
</template>
<style scoped lang="less">
.row button {
  flex-grow: 1;
}

button {
  cursor: pointer;
  background: var(--text-color);
  outline: none;
  border: none;
  padding: 5px 10px;
  opacity: .8;

  &.active {
    opacity: 1;
  }

  svg {
    stroke: var(--base-color);
    width: 45px;
  }
}
</style>