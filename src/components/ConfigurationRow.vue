<template>
  <div class="row">
    <label :for="label">{{ label }}:</label>
    <input type="range" :id="label" :min="min" :max="max" :step="step" :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)">
    <i :value="modelValue"></i>
  </div>
</template>

<script setup>
defineProps(['modelValue', 'label', 'min', 'max', 'step'])
defineEmits(['update:modelValue'])
</script>

<style scoped lang="less">
.row {
  @media screen and (max-width:800px) {

    position: unset;
  }
}


input[type=range] {
  appearance: none;
  cursor: pointer;
  width: 100%;
  margin-top: 3px;
  background-color: transparent;
}

/** ,选择器不生效 只能单独选择 */
/** input range track common css */
.track {
  width: 100%;
  height: 6px;
  cursor: pointer;
  background: var(--text-color);
  border-radius: 3px
}

input[type=range]::-webkit-slider-runnable-track {
  .track()
}

input[type=range]::-moz-range-track {
  .track()
}


/** input range thumb common css */
.thumb {
  box-shadow: 1px 1px 1px #30302f, 0 0 1px #30302f;
  border: 1px solid #000;
  height: 16px;
  width: 16px;
  border-radius: 50%;
  background: var(--text-color);
  cursor: pointer;
}

input[type=range]::-webkit-slider-thumb {
  appearance: none;
  margin-top: -5px;
  .thumb()
}

input[type=range]::-ms-thumb {
  .thumb()
}

input[type=range]::-moz-range-thumb {
  .thumb()
}

// input[type=range] {

//   &::-webkit-slider-runnable-track,
//   &::-moz-range-track,
//   &::-ms-track {
//     width: 100%;
//     height: 6px;
//     cursor: pointer;
//     background: var(--text-color);
//     border-radius: 3px
//   }

//   &::-webkit-slider-thumb,
//   &::-moz-range-thumb,
//   &::-ms-thumb {
//     box-shadow: 1px 1px 1px #30302f, 0 0 1px #30302f;
//     border: 1px solid #000;
//     height: 16px;
//     width: 16px;
//     border-radius: 50%;
//     background: #001f3f;
//     background: var(--text-color);
//     cursor: pointer;
//     margin-top: -5px
//   }
//   &::-webkit-slider-thumb{    
//     -webkit-appearance: none;
//   }

// }

input[type=range] {

  &:hover+i,
  &:active+i,
  &:focus+i,
  &:focus-within+i {
    &::before {
      width: 0;
      height: 0;
      content: '';
      border-right: 15px solid var(--text-color);
      border-top: 10px solid transparent;
      border-bottom: 10px solid transparent;
      position: absolute;
      left: 103%;
      top: 50%;
      transform: translateY(-50%);

      @media screen and(max-width:800px) {
        display: none;
      }
    }

    &::after {
      content: attr(value) "px";
      position: absolute;
      left: 105%;
      top: 50%;
      transform: translateY(-50%);
      color: var(--base-color);
      border-radius: 3px;
      padding: 10px;
      background: var(--text-color);

      @media screen and(max-width:800px) {
        right: 20px;
        top: 22px;
        left: unset;
        transform: unset;
      }
    }


  }

}
</style>