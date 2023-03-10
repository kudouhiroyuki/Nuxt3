<template>
  <HeadlessListbox v-model="text" :disabled="disabled" v-slot="{ open }">
    <div
      ref="referenceRef"
      class="select uc-icon-textbox uc-icon-textbox--has-right-icon"
      :class="[
        {
          'w-full': block,
        },
        `uc-icon-textbox--h${size}`,
      ]"
      v-bind="$attrs"
    >
      <HeadlessListboxButton
        class="uc-textbox text-left bg-white"
        :class="[
          {
            'uc-textbox--disabled': disabled,
            'uc-textbox--error': error,
            'w-full': block,
            '!h-auto py-1.5': multiLine,
            'overflow-hidden whitespace-nowrap text-ellipsis': !multiLine,
          },
        ]"
      >
        <span>{{ displayLabel }}</span>
      </HeadlessListboxButton>
      <Icon
        class="
          uc-icon-textbox__icon uc-icon-textbox__icon--right
          text-primary-500
        "
        :class="{
          '!text-neutral-disable': disabled,
          '!text-error-500': error,
        }"
        :icon="mdiMenuDown"
      />
    </div>

    <div v-if="open" class="select__backdrop" aria-hidden="true"></div>

    <transition
      enter-active-class="transition duration-150 ease-out"
      enter-from-class="opacity-0"
      enter-to-class="opacity-100"
      leave-active-class="transition duration-150 ease-in"
      leave-from-class="opacity-100"
      leave-to-class="opacity-0"
    >
      <div
        ref="floatingRef"
        class="
          shadow-2xl
          bg-white
          py-2
          z-50
          min-w-40
          text-m
          max-w-11/12
          overflow-auto
        "
        v-if="open"
        :style="{
          position: strategy,
          top: `${y ?? 0}px`,
          left: `${x ?? 0}px`,
          width: 'max-content',
          maxHeight: 'calc(100vh - 32px)',
        }"
      >
        <HeadlessListboxOptions static>
          <slot></slot>
        </HeadlessListboxOptions>
      </div>
    </transition>
  </HeadlessListbox>
</template>
<script setup lang="ts">
import { useSlots, ref } from "vue";
import {
  useFloating,
  autoUpdate,
  offset,
  inline,
  flip,
  shift,
  detectOverflow,
} from "@floating-ui/vue";
import { mdiMenuDown } from "@mdi/js";

// --------------------------------------------------------------------------
// props
// --------------------------------------------------------------------------
interface Props {
  /** ?????????????????????????????? @type { '2l' | 'l' | 'm' | 's'} */
  size?: "2l" | "l" | "m" | "s" | "auto";
  /** v-model?????????????????????value @type {any} */
  modelValue?: any;
  /** ????????????????????? @type {string} */
  currentValue?: string;
  /** ?????? @type {boolean} */
  disabled?: boolean;
  /** ????????? @type {boolean} */
  error?: boolean;
  /** ???100% @type {boolean} */
  block?: boolean;
  /** false: ???????????????1?????????...??????????????? @type {boolean} */
  multiLine?: boolean;
  /** ???????????????????????????. ???????????????????????????????????? @type {boolean} */
  unSelectedLabel?: string;
}

const props = withDefaults(defineProps<Props>(), {
  size: "m",
  modelValue: null,
  disabled: false,
  error: false,
  multiLine: false,
  unSelectedLabel: "????????????????????????",
});

const emit =
  defineEmits<{
    (e: "update:modelValue", value: any): void;
  }>();

const text = computed({
  get: () => props.modelValue,
  set: (value) => {
    // ????????????????????????????????????setter
    emit("update:modelValue", value);
  },
});

const slots = useSlots();

const displayLabel = computed(() => {
  if (!props.modelValue) return props.unSelectedLabel;

  return props.currentValue ?? props.modelValue;
});

/**
 * Tooltip
 */
const referenceRef = ref<HTMLDivElement>();
const floatingRef = ref<HTMLDivElement>();

const { x, y, strategy } = useFloating(referenceRef, floatingRef, {
  placement: "bottom-start",
  strategy: "fixed",
  middleware: [
    offset(4),
    inline(),
    shift({
      padding: 16,
      crossAxis: true,
    }),
    {
      name: "middleware",
      async fn(middlewareArguments) {
        // ????????????????????????????????????????????????????????????
        const overflow = await detectOverflow(middlewareArguments);

        // ???????????????????????????????????????????????????.(??????????????????????????????????????????????????????)
        if (overflow.top <= 0 && overflow.bottom <= 0) return {};

        const { height: refHeight, y: refY } =
          middlewareArguments.rects.reference;
        const { height: floatingHeight, y: floatingY } =
          middlewareArguments.rects.floating;
        const { innerHeight } = window;

        const y =
          innerHeight < refY + refHeight + floatingHeight
            ? innerHeight - floatingHeight - 8
            : refY + refHeight;

        return { y };
      },
    },
  ],
  whileElementsMounted: (reference, floating, update) => {
    autoUpdate(reference, floating, () => {
      update();
    });
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
.select {
}

// ??????
.select__backdrop {
  position: fixed;
  z-index: 50;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background: transparent;
}
</style>
