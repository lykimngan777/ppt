<template>
  <span
    class="switch"
    :class="{
      'active': value,
      'disabled': disabled,
    }" 
    @click="handleChange()"
  >
    <span class="switch-core"></span>
  </span>
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

const handleChange = () => {
  if (props.disabled) return
  emit('update:value', !props.value)
}
</script>

<style lang="scss" scoped>
.switch {
  height: 20px;
  display: inline-block;
  cursor: pointer;

  &:not(.disabled).active {
    .switch-core {
      border-color: #0288D1;
      background-color: #0288D1;

      &::after {
        left: 100%;
        margin-left: -17px;
      }
    }
  }

  &.disabled {
    cursor: default;
    opacity: 0.6;

    .switch-core::after {
      background-color: #666;
    }
  }
}
.switch-core {
  margin: 0;
  display: inline-block;
  position: relative;
  width: 40px;
  height: 20px;
  border: 1px solid #444;
  outline: none;
  border-radius: 10px;
  box-sizing: border-box;
  background: #333;
  transition: all $transitionDelay;
  vertical-align: middle;

  &::after {
    content: '';
    position: absolute;
    top: 1px;
    left: 1px;
    border-radius: 100%;
    transition: all .3s;
    width: 16px;
    height: 16px;
    background-color: #fff;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.4);
  }
}
</style>