<template>
  <ul class="menu-content">
    <template v-for="(menu, index) in menus" :key="menu.text || index">
      <li
        v-if="!menu.hide"
        class="menu-item"
        @click.stop="handleClickMenuItem(menu)"
        :class="{'divider': menu.divider, 'disable': menu.disable}"
      >
        <div 
          class="menu-item-content" 
          :class="{
            'has-children': menu.children,
            'has-handler': menu.handler,
          }" 
          v-if="!menu.divider"
        >
          <span class="text">{{menu.text}}</span>
          <span class="sub-text" v-if="menu.subText && !menu.children">{{menu.subText}}</span>

          <menu-content 
            class="sub-menu"
            :menus="menu.children" 
            v-if="menu.children && menu.children.length"
            :handleClickMenuItem="handleClickMenuItem" 
          />
        </div>
      </li>
    </template>
  </ul>
</template>

<script lang="ts" setup>
import type { ContextmenuItem } from './types'

defineProps<{
  menus: ContextmenuItem[]
  handleClickMenuItem: (item: ContextmenuItem) => void
}>()
</script>

<style lang="scss" scoped>
$menuWidth: 180px;
$menuHeight: 30px;
$subMenuWidth: 120px;

.menu-content {
  width: $menuWidth;
  padding: 5px 0;
  background: $sidebarBg;
  border: 1px solid $borderColor;
  box-shadow: $boxShadow;
  border-radius: $borderRadius;
  list-style: none;
  margin: 0;
}
.menu-item {
  padding: 0 15px;
  color: $textColor;
  font-size: 12px;
  transition: all $transitionDelayFast;
  white-space: nowrap;
  height: $menuHeight;
  line-height: $menuHeight;
  background-color: transparent;
  cursor: pointer;

  &:not(.disable):hover > .menu-item-content > .sub-menu {
    display: block;
  }

  &:not(.disable):hover > .has-children.has-handler::after {
    transform: scale(1);
  }

  &:hover:not(.disable) {
    background-color: #2D2D2D;
    color: #fff;
  }

  &.divider {
    height: 1px;
    overflow: hidden;
    margin: 4px 0;
    background-color: $borderColor;
    line-height: 0;
    padding: 0;
  }

  &.disable {
    color: #555;
    cursor: no-drop;
  }
}
.menu-item-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: relative;

  &.has-children::before {
    content: '';
    display: inline-block;
    width: 6px;
    height: 6px;
    border-width: 1px;
    border-style: solid;
    border-color: #888 #888 transparent transparent;
    position: absolute;
    right: 0;
    top: 50%;
    transform: translateY(-50%) rotate(45deg);
  }
  &.has-children.has-handler::after {
    content: '';
    display: inline-block;
    width: 1px;
    height: 20px;
    background-color: $borderColor;
    position: absolute;
    right: 18px;
    top: 5px;
    transform: scale(0);
    transition: transform $transitionDelayFast;
  }

  .sub-text {
    color: #888;
    margin-left: 20px;
    font-size: 11px;
  }
  .sub-menu {
    width: $subMenuWidth;
    position: absolute;
    display: none;
    left: 100%;
    margin-left: 5px;
    top: -6px;
  }
}
</style>