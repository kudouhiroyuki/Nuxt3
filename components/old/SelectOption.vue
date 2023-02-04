<template>
  <HeadlessListboxOption
    :value="value"
    :disabled="disabled"
    v-slot="{ active, selected }"
    as="template"
  >
    <li
      class="
        relative
        cursor-pointer
        py-1.5
        pr-2
        pl-8
        text-left
        hover:bg-primary-100
      "
      :class="{
        'bg-primary-100': selected,
        'bg-neutral-alt-background': active,
        'text-neutral-disable cursor-default hover:bg-transparent': disabled,
      }"
    >
      <Icon
        v-if="selected"
        class="text-primary-500 align-middle text-18 absolute left-2 top-1.5"
        :icon="mdiCheck"
      />
      <span class="align-middle">
        <slot></slot>
      </span>
    </li>
  </HeadlessListboxOption>
</template>
<script setup lang="ts">
import { mdiCheck } from "@mdi/js";

// --------------------------------------------------------------------------
// props
// --------------------------------------------------------------------------
interface Props {
  /** value @type {any} */
  value?: any;
  /** 無効 @type {boolean} */
  disabled?: boolean;
  /** エラー @type {boolean} */
  error?: boolean;
  /** 幅100% @type {boolean} */
  block?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  variant: "default",
  size: "m",
  modelValue: "",
  disabled: false,
  error: false,
});

const emit =
  defineEmits<{
    (e: "update:modelValue", value: string): void;
  }>();
</script>
