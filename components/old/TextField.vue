<template>
  <div
    class="uc-icon-textbox"
    :class="[
      {
        'w-full': block,
        'uc-icon-textbox--has-left-icon': slots.prependIcon,
        'uc-icon-textbox--has-right-icon': slots.appendIcon,
      },
      `uc-icon-textbox--h${sizeInput}`,
      props.class
    ]"
  >
    <span
      class="uc-icon-textbox__icon uc-icon-textbox__icon--left leading-none"
      v-if="slots.prependIcon"
    >
      <slot name="prependIcon"></slot>
    </span>
    <input
      class="uc-textbox"
      :class="[
        {
          'uc-textbox--disabled': disabled,
          'uc-textbox--readonly': readonly,
          'uc-textbox--error': error,
          'w-full': block,
        },
        inputClass
      ]"
      type="text"
      v-model="text"
      :disabled="disabled || readonly"
      v-bind="attrs"
    />
    <span
      class="uc-icon-textbox__icon uc-icon-textbox__icon--right leading-none"
      v-if="slots.appendIcon"
    >
      <slot name="appendIcon"></slot>
    </span>
    <Button
      class="uc-icon-textbox__icon-button uc-icon-textbox__icon-button--right"
      @click="onClickIcon"
      v-if="slots.buttonIcon"
    >
      <template v-slot:appendIcon>
        <slot name="buttonIcon"></slot>
      </template>
    </Button>
  </div>
</template>
<script setup lang="ts">
// --------------------------------------------------------------------------
// props
// --------------------------------------------------------------------------
interface Props extends Partial<HTMLInputElement> {
  class?: string | object;
  modelValue?: string;
  disabled?: boolean;
  readonly?: boolean;
  error?: boolean;
  block?: boolean;
  inputClass?: string | object;
  /** ボタンのサイズ @type {'3l' | '2l' | 'l' | 'm' | 's'} */
  sizeInput?: "3l" | "2l" | "l" | "m" | "s";
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: "",
  disabled: false,
  error: false,
  sizeInput: "m",
});

const slots = useSlots();
const attrs = useAttrs();

const emit =
  defineEmits<{
    (e: "update:modelValue", text: string): void;
    (e: "clickIcon", event: MouseEvent): void;
  }>();

const text = computed({
  get: () => props.modelValue,
  set: (value) => {
    emit("update:modelValue", value);
  },
});

const onClickIcon = (event: MouseEvent) => {
  emit("clickIcon", event);
};
</script>
