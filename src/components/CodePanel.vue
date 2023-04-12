<script setup>
import { ref } from 'vue'
import IconCopy from './svg/IconCopy.vue';
const props = defineProps(['codeString'])
const copying = ref(false)
/** copy to clipboard  */
const copyToClipboard = async () => {
  await window.navigator.clipboard.writeText(props.codeString)
  copying.value = true
  setTimeout(() => {
    copying.value = false
  }, 1000);
  console.log('copy to clipboard')
}
</script>

<template>
  <div class="code-panel" :class="{ copying }">
    <button @click="copyToClipboard">
      <IconCopy></IconCopy>
    </button>
    <pre><code>{{ props.codeString }}</code></pre>
    <Transition>
      <div v-show="copying" class="mask" :class="{ copying }">
        <div class="content">Copied!</div>
      </div>
    </Transition>
  </div>
</template>

<style scoped lang='less'>
.code-panel {
  position: relative;
  border-radius: 6px;
  padding: 10px 8px;
  background: rgba(48, 228, 87, .8);

  @media screen and (max-width:800px) {
    font-size: 11px;
  }

  @media screen and (min-width:1280px) {
    min-width: 420px;
  }

  button {
    border: 0;
    padding: 5px;
    position: absolute;
    right: 5px;
    border-radius: 5px;
    top: 5px;
    cursor: pointer;
    opacity: 0;
    transition: opacity .4s;
  }

  &:hover {
    button {
      opacity: 1;
    }
  }

  .mask {
    width: 100%;
    height: 100%;
    position: absolute;
    left: 0;
    top: 0;
    display: flex;
    align-items: center;
    justify-content: center;

    .content {
      border-radius: 6px;
      background: #47a049;
      width: 100%;
      height: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
    }
  }
}

.v-enter-active,
.v-leave-active {
  transition: opacity .4s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>