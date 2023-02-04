<template>
  <HeadlessSwitch
    v-model="checked"
    class="uc-checkbox checkbox"
    :class="[
      {
        'checkbox--checked': checked,
        'uc-checkbox--outlined': variant === 'outlined',
        'uc-checkbox--disabled': disabled,
        'uc-checkbox--error': error,
        [`uc-checkbox--h${size}`]: size !== 'auto',
        [`checkbox--auto`]: size === 'auto',
        'w-full': block,
      },
    ]"
    :disabled="disabled"
    v-bind="$attrs"
  >
    <input
      type="checkbox"
      :checked="checked"
      :disabled="disabled"
      tabindex="-1"
    />
    <label class="uc-checkbox__label">
      <Icon
        class="checkbox__icon"
        :class="{ 'checkbox__icon--checked': checked }"
        :icon="checked ? mdiCheckboxMarked : mdiCheckboxBlankOutline"
      />
      <slot></slot>
    </label>
  </HeadlessSwitch>
</template>
<script setup lang="ts">
import { mdiCheckboxBlankOutline, mdiCheckboxMarked } from "@mdi/js";

// --------------------------------------------------------------------------
// props
// --------------------------------------------------------------------------
interface Props {
  variant?: "default" | "outlined";
  size?: "2l" | "l" | "m" | "s" | "auto";
  modelValue?: boolean;
  disabled?: boolean;
  error?: boolean;
  block?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  variant: "default",
  size: "m",
  modelValue: false,
  disabled: false,
  error: false,
});

const emit =
  defineEmits<{
    (e: "update:modelValue", checked: boolean): void;
  }>();

const checked = computed({
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
.checkbox {
  // UDSの上書き
  & > .uc-checkbox__label {
    white-space: nowrap;

    &::before,
    &::after {
      content: none !important;
    }
  }
}

// UDSの上書き
.uc-checkbox--disabled {
  & > .uc-checkbox__label,
  .checkbox__icon {
    color: $colors-primitive-gray-600;
    cursor: default;
  }
}

// UDSの上書き
.uc-checkbox--error {
  .checkbox__icon {
    color: $colors-error-500;
  }
}

.checkbox--auto {
  .checkbox__icon {
    top: 1.125rem;
  }

  // UDSの上書き
  & > .uc-checkbox__label {
    height: auto;
  }
}

.checkbox__icon {
  font-size: 1.125rem;
  color: $colors-neutral-placeholder;
  position: absolute;
  left: 0.375rem;
  top: 50%;
  transform: translateY(-50%);
}

.checkbox__icon--checked {
  color: $colors-primary-400;
}
</style>
