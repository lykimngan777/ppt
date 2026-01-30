<template>
  <button 
    class="button"
    :class="{
      'disabled': disabled,
      'checked': !disabled && checked,
      'default': !disabled && type === 'default',
      'primary': !disabled && type === 'primary',
      'checkbox': !disabled && type === 'checkbox',
      'radio': !disabled && type === 'radio',
      'small': size === 'small',
      'first': first,
      'last': last,
    }"
    @click="handleClick()"
  >
    <slot></slot>
  </button>
</template>

<script lang="ts" setup>
const props = withDefaults(defineProps<{
  checked?: boolean
  disabled?: boolean
  type?: 'default' | 'primary' | 'checkbox' | 'radio'
  size?: 'small' | 'normal'
  first?: boolean
  last?: boolean
}>(), {
  checked: false,
  disabled: false,
  type: 'default',
  size: 'normal',
  first: false,
  last: false,
})

const emit = defineEmits<{
  (event: 'click'): void
}>()

const handleClick = () => {
  if (props.disabled) return
  emit('click')
}
</script>

<style lang="scss" scoped>
.button {
  height: 32px;
  line-height: 32px;
  outline: 0;
  font-size: 13px;
  padding: 0 15px;
  text-align: center;
  color: $textColor;
  border-radius: $borderRadius;
  user-select: none;
  letter-spacing: 1px;
  cursor: pointer;

  &.small {
    height: 24px;
    line-height: 24px;
    padding: 0 7px;
    letter-spacing: 0;
    font-size: 12px;
  }

  &.default {
    background-color: #2D2D2D;
    border: 1px solid $borderColor;
    color: $textColor;

    &:hover {
      background-color: #3D3D3D;
      border-color: $accentColor;
    }
  }
  &.primary {
    background-color: #374151;
    border: 1px solid #374151;
    color: #fff;

    &:hover {
      background-color: #4B5563;
      border-color: #4B5563;
    }
  }
  &.checkbox, &.radio {
    background-color: #2D2D2D;
    border: 1px solid $borderColor;
    color: $textColor;

    &:not(.checked):hover {
      background-color: #3D3D3D;
    }
  }
  &.checked {
    color: #fff;
    background-color: #0288D1;
    border-color: #0288D1;

    &:hover {
      background-color: #039BE5;
      border-color: #039BE5;
    }
  }
  &.disabled {
    background-color: #1A1A1A;
    border: 1px solid #333;
    color: #555;
    cursor: default;
  }
}
</style>