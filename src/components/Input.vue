<template>
  <div 
    class="input"
    :class="{
      'disabled': disabled,
      'focused': focused,
      'simple': simple,
    }"
  >
    <span class="prefix">
      <slot name="prefix"></slot>
    </span>
    <input
      type="text"
      ref="inputRef"
      :disabled="disabled"
      :value="value" 
      :placeholder="placeholder"
      :maxlength="maxlength"
      @input="$event => handleInput($event)"
      @focus="$event => handleFocus($event)"
      @blur="$event => handleBlur($event)"
      @change="$event => emit('change', $event)"
      @keydown.enter="$event => emit('enter', $event)"
      @keydown.backspace="$event => emit('backspace', $event)"
    />
    <span class="suffix">
      <slot name="suffix"></slot>
    </span>
  </div>
</template>

<script lang="ts" setup>
import { useTemplateRef, ref } from 'vue'

withDefaults(defineProps<{
  value: string
  disabled?: boolean
  placeholder?: string
  simple?: boolean
  maxlength?: number
}>(), {
  disabled: false,
  placeholder: '',
  simple: false,
})

const emit = defineEmits<{
  (event: 'update:value', payload: string): void
  (event: 'input', payload: Event): void
  (event: 'change', payload: Event): void
  (event: 'blur', payload: Event): void
  (event: 'focus', payload: Event): void
  (event: 'enter', payload: Event): void
  (event: 'backspace', payload: Event): void
}>()

const focused = ref(false)

const handleInput = (e: Event) => {
  emit('update:value', (e.target as HTMLInputElement).value)
}
const handleBlur = (e: Event) => {
  focused.value = false
  emit('blur', e)
}
const handleFocus = (e: Event) => {
  focused.value = true
  emit('focus', e)
}

const inputRef = useTemplateRef<HTMLInputElement>('inputRef')
const focus = () => {
  if (inputRef.value) inputRef.value.focus()
}

defineExpose({
  focus,
})
</script>

<style lang="scss" scoped>
.input {
  background-color: #2D2D2D;
  border: 1px solid $borderColor;
  padding: 0 8px;
  border-radius: $borderRadius;
  transition: all $transitionDelay;
  font-size: 13px;
  display: flex;
  color: $textColor;

  input {
    min-width: 0;
    height: 32px;
    outline: 0;
    border: 0;
    line-height: 32px;
    vertical-align: top;
    color: inherit;
    background-color: transparent;
    padding: 0;
    flex: 1;
    font-size: 13px;
    font-family: inherit;

    &::placeholder {
      color: #666;
    }
  }

  &:not(.disabled):hover, &.focused {
    border-color: $accentColor;
    background-color: #3D3D3D;
  }

  &.disabled {
    background-color: #1A1A1A;
    border-color: #333;
    color: #555;

    input {
      color: #555;
    }
  }

  &.simple {
    border: 0;
    background-color: transparent;
  }

  .prefix, .suffix {
    display: flex;
    justify-content: center;
    align-items: center;
    line-height: 32px;
    user-select: none;
    color: #888;
  }
}
</style>