<template>
  <span
    class="uc-badge"
    :class="{
      'uc-badge--no-text': noText,
      'uc-badge--info': type === 'info',
      'uc-badge--success': type === 'success',
      'uc-badge--warning': type === 'warning',
      'uc-badge--important': type === 'important',
      'uc-badge--light': type === 'light',
    }"
  >
    <slot></slot>
  </span>
</template>

<script setup lang="ts">
interface Props {
  noText?: boolean;
  type?: "info"|"success"|"warning"|"important"|"light";
}

const props = withDefaults(defineProps<Props>(), {
  noText: false,
});
</script>
