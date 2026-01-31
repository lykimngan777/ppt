<template>
  <textarea
    class="textarea" 
    :class="{
      'disabled': disabled,
      'resizable': resizable,
    }"
    ref="textareaRef"
    :disabled="disabled"
    :value="value" 
    :rows="rows"
    :placeholder="placeholder"
    :style="{
      padding: padding ? `${padding}px` : '10px',
    }"
    @input="$event => handleInput($event)"
    @focus="$event => emit('focus', $event)"
    @blur="$event => emit('blur', $event)"
    @keydown.enter="$event => emit('enter', $event)"
  ></textarea>
</template>

<script lang="ts" setup>
import { useTemplateRef } from 'vue'

withDefaults(defineProps<{
  value: string
  rows?: number
  padding?: number
  disabled?: boolean
  resizable?: boolean
  placeholder?: string
}>(), {
  rows: 4,
  disabled: false,
  resizable: false,
  placeholder: '',
})

const emit = defineEmits<{
  (event: 'update:value', payload: string): void
  (event: 'focus', payload: FocusEvent): void
  (event: 'blur', payload: FocusEvent): void
  (event: 'enter', payload: KeyboardEvent): void
}>()

const handleInput = (e: Event) => {
  emit('update:value', (e.target as HTMLInputElement).value)
}

const textareaRef = useTemplateRef<HTMLTextAreaElement>('textareaRef')
const focus = () => {
  if (textareaRef.value) textareaRef.value.focus()
}

defineExpose({
  focus,
})
</script>

<style lang="scss" scoped>
.textarea {
  outline: 0;
  width: 100%;
  background-color: $charcoal;
  border: 1px solid $borderColor;
  border-radius: $borderRadius;
  padding: 10px;
  transition: all $transitionDelay;
  box-sizing: border-box;
  line-height: 1.6;
  resize: none;
  font-family: inherit;
  color: $textColor;

  &:focus {
    border-color: $accentColor;
    background-color: #FFFFFF;
  }

  &.resizable {
    resize: vertical;
  }

  &.disabled {
    background-color: rgba($charcoal, 0.5);
    border-color: $borderColor;
    color: #999;
  }

  &::placeholder {
    color: #999;
  }
}
</style>