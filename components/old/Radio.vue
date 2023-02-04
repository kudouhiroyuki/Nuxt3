<template>
  <HeadlessRadioGroupOption
    class="inline-flex"
    :class="{
      'w-full': block,
    }"
    v-slot="{ active, checked }"
    :disabled="disabled"
    v-bind="$attrs"
  >
    <div
      class="uc-radio radio"
      :class="[
        {
          'radio--checked': checked,
          'uc-radio--outlined': variant === 'outlined',
          'uc-radio--disabled': disabled,
          'uc-radio--error': error,
          'uc-radio--block': block,
          [`uc-radio--h${size}`]: size !== 'auto',
          [`radio--auto`]: size === 'auto',
          'w-full': block,
        },
      ]"
    >
      <input type="checkbox" :checked="checked" :disabled="disabled" tabindex="-1" />
      <div class="uc-radio__label">
        <Icon
          class="radio__icon"
          :class="{ 'radio__icon--checked': checked }"
          :icon="checked ? mdiRadioboxMarked : mdiRadioboxBlank"
        />
        <slot></slot>
      </div>
    </div>
  </HeadlessRadioGroupOption>
</template>
<script setup lang="ts">
import { mdiRadioboxBlank, mdiRadioboxMarked } from "@mdi/js";

// --------------------------------------------------------------------------
// props
// --------------------------------------------------------------------------
interface Props {
  /** ラジオボタンの種類 @type {'default'|'outlined'} */
  variant?: "default" | "outlined";
  /** ラジオボタンのサイズ @type { '2l' | 'l' | 'm' | 's'} */
  size?: "2l" | "l" | "m" | "s" | "auto";
  /** v-modelを使用する際のvalue @type {string} */
  modelValue?: string;
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

const text = computed({
  get: () => props.modelValue,
  set: (value) => {
    // 値に変更があると呼ばれるsetter
    emit("update:modelValue", value);
  },
});
</script>
<script lang="ts">
import { defineComponent } from "@vue/runtime-core";

export default defineComponent({
  inheritAttrs: false,
});
</script>
<style lang="scss">
.radio {

// UDSの上書き
& > .uc-radio__label {
  white-space: nowrap;

  &::before,
  &::after {
    content: none;
  }
}
}

// UDSの上書き
.uc-radio--disabled {
& > .uc-radio__label,
.radio__icon {
  color: #bfbfbf;
  cursor: default;
}
}

// UDSの上書き
.uc-radio--error {
.radio__icon {
  color: #e32323;
}
}

.radio--auto {

.radio__icon {
  top: 1.125rem;
}

// UDSの上書き
& > .uc-radio__label {
  height: auto;
}
}

.radio__icon {
font-size: 1.125rem;
color: #999999;
position: absolute;
left: 0.375rem;
top: 50%;
transform: translateY(-50%);
}

.radio__icon--checked {
color: #4785fb;
}

</style>
