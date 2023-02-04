<template>
  <HeadlessPopover
    class="tooltip"
    :class="{
      'tooltip--error': error,
    }"
    v-slot="{ open: _open, close }"
  >
    <span ref="referenceRef" class="tooltip__trigger">
      <HeadlessPopoverButton class="" v-if="slots.trigger">
        <slot name="trigger" :open="_open"></slot>
      </HeadlessPopoverButton>
      <slot></slot>
    </span>

    <transition
      enter-active-class="transition duration-150 ease-out z-10"
      enter-from-class="opacity-0"
      enter-to-class="opacity-100"
      leave-active-class="transition duration-150 ease-in z-10"
      leave-from-class="opacity-100"
      leave-to-class="opacity-0"
      :on-before-enter="onShowTooltip"
    >
      <HeadlessPopoverPanel v-show="_open || open" static>
        <div
          ref="floatingRef"
          class="tooltip__tips"
          :style="{
            position: strategy,
            top: `${y ?? 0}px`,
            left: `${x ?? 0}px`,
            width: 'max-content',
          }"
        >
          {{ tips }}
          <slot name="tips" :open="_open || open" :close="close"></slot>

          <!-- 矢印 -->
          <div
            class="tooltip__arrow"
            ref="floatingArrowRef"
            :style="{
              position: 'absolute',
              [arrowYSide]: `${arrowY ?? 0}px`,
              [arrowXSide]: `${arrowX ?? 0}px`,
              [staticSide]: `-3px`,
            }"
          ></div>
        </div>
      </HeadlessPopoverPanel>
    </transition>
  </HeadlessPopover>
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
  arrow,
  computePosition,
  Placement,
} from "@floating-ui/vue";

interface Props {
  open?: boolean;
  tips?: string;
  place?: Placement;
  error?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  open: false,
  place: "top-start",
  tips: "",
});

const slots = useSlots();

/**
 * Tooltip
 */
const referenceRef = ref<HTMLSpanElement>();
const floatingRef = ref<HTMLDivElement>();
const floatingArrowRef = ref<HTMLDivElement>();

const arrowX = ref(0);
const arrowY = ref(0);
const staticSide = ref("bottom");
const arrowXSide = ref("left");
const arrowYSide = ref("bottom");

const computeArrowPosition = (
  reference: HTMLElement,
  floating: HTMLElement
) => {
  computePosition(reference, floating, {
    placement: props.place,
    strategy: "fixed",
    middleware: [flip(), arrow({ element: floatingArrowRef })],
  }).then(({ placement, middlewareData }) => {
    const place = placement.split("-")[0];
    staticSide.value = {
      top: "bottom",
      right: "left",
      bottom: "top",
      left: "right",
    }[place] as string;

    arrowXSide.value = place === "left" ? "right" : "left";
    arrowX.value = middlewareData?.arrow?.x ?? 0;
    arrowYSide.value = place === "right" || place === "left" ? "top" : "bottom";
    arrowY.value = middlewareData?.arrow?.y ?? 0;
  });
};

const { x, y, strategy, update } = useFloating(referenceRef, floatingRef, {
  placement: props.place,
  strategy: "fixed",
  middleware: [
    offset(4),
    inline(),
    flip(),
    shift({ padding: 5 }),
    arrow({ element: floatingArrowRef }),
  ],
  whileElementsMounted: (reference, floating, update) => {
    autoUpdate(reference, floating, () => {
      update();

      computeArrowPosition(reference, floating);
    });
  },
});

watch(
  () => props.open,
  () => {
    update();
  }
);

const onShowTooltip = () => {
  if (referenceRef.value && floatingRef.value && floatingArrowRef.value) {
    computeArrowPosition(referenceRef.value, floatingRef.value);
  }
};
</script>
<style lang="scss">
.tooltip {
  display: inline-block;
}

.tooltip--error {
  .tooltip__tips {
    font-weight: bold;
    background: $colors-error-500;
  }

  .tooltip__arrow {
    background: $colors-error-500;
  }
}

.tooltip__trigger {
  display: inline-block;
}

.tooltip__tips {
  z-index: 10;
  background: $colors-primitive-gray-900;
  color: #fff;
  padding: 0.5rem;
  border-radius: 0.125rem;
  max-width: 20rem;
  font-size: 0.625rem;
}

.tooltip__arrow {
  width: 0.375rem;
  height: 0.375rem;
  background-color: $colors-primitive-gray-900;
  position: absolute;
  transform: rotate(45deg);
}

</style>
