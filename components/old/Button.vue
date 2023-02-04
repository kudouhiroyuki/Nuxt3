<template>
  <component
    :to="!disabled && !loading ? to : undefined"
    :href="!disabled && !loading ? href : undefined"
    :target="target"
    class="uc-button"
    :class="[
      {
        'uc-button--primary': variant === 'primary',
        'uc-button--outlined': variant === 'outlined',
        'uc-button--danger': distructive,
        'uc-button--secondary': variant === 'secondary',
        'uc-button--text': variant === 'text',
        'uc-button--disabled': disabled || loading,
        'w-full': !!block,
      },
      `uc-button--h${size}`,
    ]"
    @click="onClick"
    :disabled="disabled || loading"
  >
    <span class="uc-button__icon leading-none" v-if="slots.prependIcon">
      <slot name="prependIcon"></slot>
    </span>
    <span class="uc-button__icon leading-none is-rotate" v-if="loading">
      <Icon class="" :icon="mdiLoading" />
    </span>
    <span class="uc-button__label" v-if="slots.default">
      <slot></slot>
    </span>
    <span class="uc-button__icon leading-none" v-if="slots.appendIcon">
      <slot name="appendIcon"></slot>
    </span>
  </component>
</template>

<script setup lang="ts">
import { computed, useSlots } from "vue";
import { mdiLoading } from "@mdi/js";

const emits = defineEmits(["click"]);
const slots = useSlots();

// --------------------------------------------------------------------------
// props
// --------------------------------------------------------------------------
interface Props extends Partial<HTMLButtonElement> {
  variant?: "primary" | "secondary" | "outlined" | "text";
  block?: boolean;
  distructive?: boolean;
  disabled?: boolean;
  loading?: boolean;
  size?: "3l" | "2l" | "l" | "m" | "s";
  nuxt?: boolean;
  href?: string | object;
  to?: string | object;
  target?: string;
  as?: string;
}

const props = withDefaults(defineProps<Props>(), {
  variant: "primary",
  size: "m",
  class: "",
  block: false,
  disabled: false,
  loading: false,
  prependIcon: undefined,
  appendIcon: undefined,
  click: undefined,
  nuxt: false,
  href: undefined,
  to: undefined,
});

const component = computed(() => {
  // タグの指定
  if (props.nuxt) return resolveComponent("NuxtLink");
  if (props.href) return resolveComponent("a");
  if (props.as) return resolveComponent(props.as);
  return "button";
});

const onClick = (event: Event) => {
  // 無効なら何もしない
  if (props.disabled || props.loading) return;
  emits("click", event);
};
</script>
<style lang="scss" scoped>

@keyframes rotate {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.is-rotate {
  animation: rotate;
  animation-duration: 800ms;
  animation-fill-mode: forwards;
  animation-iteration-count: infinite;
  animation-timing-function: linear;
}

</style>
