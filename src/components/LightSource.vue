<script setup>
defineProps({
  left: 'unset',
  right: 'unset',
  top: 'unset',
  bottom: 'unset',
  borderRadius: 'unset'
})
</script>

<template>
  <div class="light-source"></div>
</template>
<style scoped lang="less">
.light-source {
  position: absolute;
  left: v-bind('left');
  right: v-bind('right');
  top: v-bind('top');
  bottom: v-bind('bottom');
  height: 30px;
  width: 30px;
  background: transparent;
  cursor: pointer;
  border: 2px solid var(--text-color);
  border-radius: v-bind('borderRadius');

  &.active {
    background: #ffff00;
  }
}
</style>