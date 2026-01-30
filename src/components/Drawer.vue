<template>
  <Teleport to="body">
    <Transition :name="`drawer-slide-${placement}`"
      @afterLeave="contentVisible = false"
      @before-enter="contentVisible = true"
    >
      <div :class="['drawer', placement]" v-show="visible" :style="{ width: props.width + 'px' }">
        <div class="header">
          <slot name="title"></slot>
          <span class="close-btn" @click="emit('update:visible', false)"><IconClose /></span>
        </div>
        <div class="content" v-if="contentVisible" :style="contentStyle">
          <slot></slot>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script lang="ts" setup>
import { computed, ref, type CSSProperties } from 'vue'

const props = withDefaults(defineProps<{
  visible: boolean
  width?: number
  contentStyle?: CSSProperties
  placement?: 'left' | 'right'
}>(), {
  width: 320,
  placement: 'right',
})

const emit = defineEmits<{
  (event: 'update:visible', payload: boolean): void
}>()

const contentVisible = ref(false)

const contentStyle = computed(() => {
  return {
    width: props.width + 'px',
    ...(props.contentStyle || {})
  }
})
</script>

<style lang="scss" scoped>
.drawer {
  height: 100%;
  position: fixed;
  top: 0;
  bottom: 0;
  z-index: 5000;
  background: $sidebarBg;
  display: flex;
  flex-direction: column;
  color: $textColor;

  &.left {
    left: 0;
    box-shadow: 3px 0 6px -4px rgba(0, 0, 0, 0.5), 9px 0 28px 8px rgba(0, 0, 0, 0.3);
    border-right: 1px solid $borderColor;
  }
  &.right {
    right: 0;
    box-shadow: -3px 0 6px -4px rgba(0, 0, 0, 0.5), -9px 0 28px 8px rgba(0, 0, 0, 0.3);
    border-left: 1px solid $borderColor;
  }
}

.header {
  height: 50px;
  padding: 0 15px;
  position: relative;
  display: flex;
  align-items: center;
  border-bottom: 1px solid $borderColor;

  .close-btn {
    width: 24px;
    height: 24px;
    display: flex;
    justify-content: center;
    align-items: center;
    position: absolute;
    top: 13px;
    right: 15px;
    cursor: pointer;
    border-radius: 4px;
    transition: all $transitionDelay;

    &:hover {
      background-color: #333;
    }
  }
}
.content {
  padding: 0 15px;
  overflow: auto;
  flex: 1;
}

.drawer-slide-right-enter-active {
  animation: drawer-slide-right-enter .25s both ease;
}
.drawer-slide-right-leave-active {
  animation: drawer-slide-right-leave .25s both ease;
}
.drawer-slide-left-enter-active {
  animation: drawer-slide-left-enter .25s both ease;
}
.drawer-slide-left-leave-active {
  animation: drawer-slide-left-leave .25s both ease;
}

@keyframes drawer-slide-right-enter {
  from {
    transform: translateX(100%);
  }
}
@keyframes drawer-slide-right-leave {
  to {
    transform: translateX(100%);
  }
}
@keyframes drawer-slide-left-enter {
  from {
    transform: translateX(-100%);
  }
}
@keyframes drawer-slide-left-leave {
  to {
    transform: translateX(-100%);
  }
}
</style>