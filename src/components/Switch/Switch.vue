<script setup lang="ts">
import { ref, computed, watch } from 'vue';
import XhIcon from '../Icon/Icon.vue';

const props = defineProps({
  modelValue: {
    type: [Boolean, String, Number],
    default: false,
  },
  value: {
    type: [Boolean, String, Number],
    default: false,
  },
  inlinePrompt: {
    type: Boolean,
    default: false,
  },
  activeIcon: {
    type: [Function, Object],
    default: () => {},
  },
  inactiveIcon: {
    type: [Function, Object],
    default: () => {},
  },
  activeValue: {
    type: [Boolean, String, Number],
    default: true,
  },
  inactiveValue: {
    type: [Boolean, String, Number],
    default: false,
  },
  tabindex: {
    type: [String, Number],
  },
});

watch(
  () => props.modelValue,
  () => {
    isActive.value = true;
  }
);

watch(
  () => props.value,
  () => {
    isActive.value = false;
  }
);

const emit = defineEmits(['change']);
const isActive = ref(props.modelValue !== false);
const actualValue = computed(() =>
  isActive.value ? props.modelValue : props.value
);
const checked = computed(() => actualValue.value === props.activeValue);
const handleChange = () => {
  emit('change', checked);
};
</script>

<template>
  <div
    @click="handleChange"
    :class="['xh-switch', { 'is-checked': checked ? true : false }]"
  >
    <input
      id="xh-switch"
      class="xh-switch__input"
      type="checkbox"
      role="switch"
      name="XhSwitch"
      :tabindex="tabindex"
      :aria-checked="checked"
      @keydown.enter="handleChange"
    />
    <span class="xh-switch__core">
      <div v-if="inlinePrompt" class="xh-switch__inner">
        <template v-if="activeIcon || inactiveIcon">
          <XhIcon
            v-if="activeIcon"
            :class="['is-icon', checked ? 'is-show' : 'is-hide']"
          >
            <component :is="activeIcon" />
          </XhIcon>
          <XhIcon
            v-if="inactiveIcon"
            :class="['is-icon', !checked ? 'is-show' : 'is-hide']"
          >
            <component :is="inactiveIcon" />
          </XhIcon>
        </template>
      </div>
      <span class="xh-switch__action"></span>
    </span>
  </div>
</template>

<style lang="scss">
.xh-switch {
  display: inline-flex;
  align-items: center;
  position: relative;
  font-size: 14px;
  line-height: 20px;
  height: 32px;
  vertical-align: middle;
  --xh-switch-on-color: var(--xh-color-primary);
  --xh-switch-off-color: var(--xh-border-color);

  &.is-checked .xh-switch__core {
    border-color: var(--xh-switch-border-color, var(--xh-switch-on-color));
    background-color: var(--xh-switch-on-color);
  }

  &.is-checked .xh-switch__core .xh-switch__action {
    left: 100%;
    margin-left: calc(-1px - 16px);
    color: var(--xh-switch-on-color);
  }

  &.is-checked .xh-switch__core .xh-switch__inner {
    left: 50%;
    white-space: nowrap;
    margin-left: calc(-1px - 16px);
  }

  .xh-switch__input {
    position: absolute;
    width: 0;
    height: 0;
    opacity: 0;
    margin: 0;
    // focus-visible相比focus解决了用户使用键盘操作视图出现的样式突兀的尴尬场景,让我们有了对键盘操作视图的样式修改能力
    //https://css-tricks.com/almanac/selectors/f/focus-visible/
    &:focus-visible {
      & ~ .xh-switch__core {
        outline: 2px solid var(--xh-switch-on-color);
        outline-offset: 1px;
      }
    }
  }

  .xh-switch__core {
    margin: 0;
    display: inline-block;
    position: relative;
    width: 40px;
    height: 20px;
    border: 1px solid var(--xh-switch-border-color, var(--xh-switch-off-color));
    outline: none;
    border-radius: 10px;
    box-sizing: border-box;
    background: var(--xh-switch-off-color);
    cursor: pointer;
    transition: border-color var(--xh-transition-duration),
      background-color var(--xh-transition-duration);

    & .is-show {
      opacity: 1;
    }

    & .is-hide {
      opacity: 0;
    }

    .xh-switch__inner {
      position: absolute;
      top: 1px;
      left: 1px;
      transition: all var(--xh-transition-duration);
      width: 16px;
      height: 16px;
      display: flex;
      justify-content: center;
      align-items: center;
      left: 50%;
      white-space: nowrap;

      .is-icon {
        color: var(--xh-color-white);
        transition: opacity var(--xh-transition-duration);
        position: absolute;
        user-select: none;
      }
    }

    .xh-switch__action {
      position: absolute;
      top: 1px;
      left: 1px;
      border-radius: var(--xh-border-radius-circle);
      transition: all var(--xh-transition-duration);
      width: 16px;
      height: 16px;
      background-color: var(--xh-color-white);
      display: flex;
      justify-content: center;
      align-items: center;
      color: var(--xh-switch-off-color);
    }
  }
}
</style>
