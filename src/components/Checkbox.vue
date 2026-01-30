<template>
  <label 
    class="checkbox"
    :class="{
      'checked': value,
      'disabled': disabled,
    }"
    @change="$event => handleChange($event)"
  >
    <span class="checkbox-input"></span>
    <input class="checkbox-original" type="checkbox" :checked="value">
    <span class="checkbox-label">
      <slot></slot>
    </span>
  </label>
</template>

<script lang="ts" setup>
const props = withDefaults(defineProps<{
  value: boolean
  disabled?: boolean
}>(), {
  disabled: false,
})

const emit = defineEmits<{
  (event: 'update:value', payload: boolean): void
}>()

const handleChange = (e: Event) => {
  if (props.disabled) return
  emit('update:value', (e.target as HTMLInputElement).checked)
}
</script>

<style lang="scss" scoped>
.checkbox {
  height: 20px;
  display: flex;
  align-items: center;
  cursor: pointer;

  &:not(.disabled).checked {
    .checkbox-input {
      background-color: #0288D1;
      border-color: #0288D1;
    }
    .checkbox-input::after {
      transform: rotate(45deg) scaleY(1);
    }

    .checkbox-label {
      color: #0288D1;
    }
  }

  &:not(.disabled):hover:not(.checked) {
    .checkbox-input {
      border-color: #0288D1;
    }
  }

  &.disabled {
    color: #555;
    cursor: default;

    .checkbox-input {
      background-color: #1A1A1A;
      border-color: #333;
    }
  }
}

.checkbox-input {
  display: inline-block;
  position: relative;
  border: 1px solid #444;
  border-radius: 4px;
  width: 16px;
  height: 16px;
  background-color: #2D2D2D;
  vertical-align: middle;
  transition: all $transitionDelayFast;
  z-index: 1;

  &::after {
    content: '';
    border: 2px solid #fff;
    border-left: 0;
    border-top: 0;
    height: 9px;
    left: 4px;
    position: absolute;
    top: 1px;
    transform: rotate(45deg) scaleY(0);
    width: 6px;
    transition: transform .15s ease-in .05s;
    transform-origin: center;
  }
}
.checkbox-original {
  opacity: 0;
  outline: 0;
  position: absolute;
  margin: 0;
  width: 0;
  height: 0;
  z-index: -1;
}
.checkbox-label {
  margin-left: 5px;
  line-height: 20px;
  font-size: 13px;
  user-select: none;
}
</style>