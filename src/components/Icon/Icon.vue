<script setup lang="ts"></script>

<template>
  <i class="xh-icon">
    <slot />
  </i>
</template>

<style scoped lang="scss">
.xh-icon {
  --color: inherit;
  height: 1em;
  width: 1em;
  line-height: 1em;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  position: relative;
  fill: currentColor;
  color: var(--color);
  font-size: inherit;

  svg {
    height: 1em;
    width: 1em;
  }
}
</style>
