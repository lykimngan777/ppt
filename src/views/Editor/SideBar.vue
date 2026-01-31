<template>
  <div class="side-bar">
    <div 
      class="tab-item" 
      v-for="tab in tabs" 
      :key="tab.value"
      :class="{ 'active': activeTab === tab.value }"
      @click="setTab(tab.value)"
    >
      <component :is="tab.icon" class="icon" />
      <div class="label">{{ tab.label }}</div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { storeToRefs } from 'pinia'
import { useMainStore } from '@/store'
import {
  GraphicDesign as IconGraphicDesign,
  Components as IconComponents,
  FontSize as IconFontSize,
  UploadOne as IconUpload,
  WritingFluently as IconWritingFluently,
  ApplicationOne as IconApplicationOne,
} from '@icon-park/vue-next'

const mainStore = useMainStore()
const { sidebarState: activeTab } = storeToRefs(mainStore)

const tabs = [
  { label: 'Thiết kế', value: 'design', icon: IconGraphicDesign },
  { label: 'Thành phần', value: 'elements', icon: IconComponents },
  { label: 'Văn bản', value: 'text', icon: IconFontSize },
  { label: 'Tải lên', value: 'uploads', icon: IconUpload },
  { label: 'Vẽ', value: 'draw', icon: IconWritingFluently },
  { label: 'Ứng dụng', value: 'apps', icon: IconApplicationOne },
]

const setTab = (value: string) => {
  mainStore.setSidebarState(value)
}
</script>

<style lang="scss" scoped>
.side-bar {
  width: 72px;
  height: 100%;
  background-color: #121212;
  display: flex;
  flex-direction: column;
  padding-top: 8px;
  z-index: 10;
}

.tab-item {
  width: 100%;
  height: 72px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: #8E8E8E;
  cursor: pointer;
  transition: all 0.2s;
  gap: 4px;

  &:hover {
    color: #FFFFFF;
  }

  &.active {
    color: #FFFFFF;
    background-color: #252525;
  }

  .icon {
    font-size: 24px;
  }

  .label {
    font-size: 10px;
    font-weight: 500;
  }
}
</style>
