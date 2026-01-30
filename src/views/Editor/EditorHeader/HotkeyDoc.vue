<template>
  <div class="hotkey-doc">
    <template v-for="item in HOTKEY_DOC" :key="item.type">
      <div class="title">{{item.type}}</div>
      <div class="hotkey-item" v-for="hotkey in item.children" :key="hotkey.label">
        <template v-if="hotkey.value">
          <div class="label">{{hotkey.label}}</div>
          <div class="value">{{hotkey.value}}</div>
        </template>
        <div class="row" v-else>{{hotkey.label}}</div>
      </div>
    </template>
  </div>
</template>

<script lang="ts" setup>
import { HOTKEY_DOC } from '@/configs/hotkey' 
</script>

<style lang="scss" scoped>
.hotkey-doc {
  height: 100%;
  overflow: auto;
  font-size: 12px;
  margin: 0;
  padding: 0 0 15px;
  color: $textColor;
}
.title {
  font-size: 14px;
  font-weight: 700;
  border-bottom: 1px solid $borderColor;
  padding: 25px 0 8px 0;
  color: $textColor;

  &:first-child {
    padding-top: 0;
  }
}
.hotkey-item {
  border-bottom: 1px solid $borderColor;
  padding: 12px 0 8px 0;
  display: flex;
  align-items: center;
  color: $textColorSecondary;
}
.label {
  width: 140px;

  @include ellipsis-oneline();
}
</style>