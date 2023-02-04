<template>
  <ModalBase
    :open="open"
    :disableBackdrop="disableBackdrop"
    :hasCloseButton="hasCloseButton"
    @on-close="onClose"
  >
    <div class="uc-modal__surface" v-bind="$attrs">
      <div class="uc-modal__header">
        <HeadlessDialogTitle class="uc-modal__header-title" v-if="slots.title">
          <slot name="title"></slot>
        </HeadlessDialogTitle>
        <Button
          class="uc-modal__close-button"
          @click="onClose(false)"
          aria-label="閉じる"
        >
          <template v-slot:appendIcon>
            <Icon class="" :icon="mdiClose" />
          </template>
        </Button>
      </div>
      <HeadlessDialogDescription as="div" class="uc-modal__content" :class="[contentClass]">
        <slot></slot>
      </HeadlessDialogDescription>

      <div class="uc-modal__actions" v-if="slots.action">
        <slot name="action"></slot>
      </div>
    </div>
  </ModalBase>
</template>

<script setup lang="ts">
import { useSlots } from "vue";
import { mdiClose } from "@mdi/js";

const emits = defineEmits(["onClose"]);

// --------------------------------------------------------------------------
// props
// --------------------------------------------------------------------------
interface Props {
  /** 開いている状態かどうか @type {boolean} */
  open?: boolean;
  /** backdropをクリックした時に閉じないようにする @type {boolean} */
  disableBackdrop?: boolean;
  /** 閉じるボタンを持つかどうか @type {boolean} */
  hasCloseButton?: boolean;
  /** コンテンツのCSSクラス @type {string|object} */
  contentClass?: string|object;
}

const props = withDefaults(defineProps<Props>(), {
  open: false,
  disableBackdrop: false,
  hasCloseButton: true,
});

const slots = useSlots();

const onClose = (isOpen?: boolean) => {
  emits("onClose", isOpen);
};

</script>
<script lang="ts">
import { defineComponent } from "@vue/runtime-core";

export default defineComponent({
  inheritAttrs: false,
});
</script>
