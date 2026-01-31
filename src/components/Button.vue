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
    background-color: $charcoal;
    border: 1px solid $borderColor;
    color: $textColor;

    &:hover {
      background-color: darken($charcoal, 3%);
      border-color: $accentColor;
    }
  }
  &.primary {
    background-color: #313D4B;
    border: 1px solid #313D4B;
    color: #fff;

    &:hover {
      background-color: lighten(#313D4B, 10%);
      border-color: lighten(#313D4B, 10%);
    }
  }
  &.checkbox, &.radio {
    background-color: $charcoal;
    border: 1px solid $borderColor;
    color: $textColor;

    &:not(.checked):hover {
      background-color: darken($charcoal, 3%);
    }
  }
  &.checked {
    color: #fff;
    background-color: $themeColor;
    border-color: $themeColor;

    &:hover {
      background-color: lighten($themeColor, 5%);
      border-color: lighten($themeColor, 5%);
    }
  }
  &.disabled {
    background-color: rgba($charcoal, 0.5);
    border: 1px solid $borderColor;
    color: #999;
    cursor: default;
  }
}
</style>