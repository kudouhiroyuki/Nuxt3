<template>
  <HeadlessTransitionRoot :show="open" as="template">
    <HeadlessDialog
      :static="!disableBackdrop"
      @close="onKeyClose"
      class="modal-wrapper"
    >
      <HeadlessTransitionChild
        enter="duration-200 ease-out"
        enter-from="opacity-0"
        enter-to="opacity-100"
        leave="duration-200 ease-in"
        leave-from="opacity-100"
        leave-to="opacity-0"
      >
        <!-- Backdrop -->
        <div class="backdrop" :aria-hidden="!open"></div>
      </HeadlessTransitionChild>

      <div
        class="modal"
        :class="{
          'modal--open': open,
        }"
      >
        <HeadlessTransitionChild
          enter="duration-200 ease-out"
          enter-from="opacity-0 translate-y-2"
          enter-to="opacity-100 translate-y-0"
          leave="duration-200 ease-in"
          leave-from="opacity-100 translate-y-0"
          leave-to="opacity-0 translate-y-2"
        >
          <HeadlessDialogPanel class="h-screen modal__container">
            <slot></slot>
          </HeadlessDialogPanel>
        </HeadlessTransitionChild>
      </div>
    </HeadlessDialog>
  </HeadlessTransitionRoot>
</template>

<script setup lang="ts">
import { useSlots } from "vue";

const emits = defineEmits(["onClose"]);

// --------------------------------------------------------------------------
// props
// --------------------------------------------------------------------------
interface Props {
  /** 開いている状態かどうか @type {boolean} */
  open?: boolean;
  /** backdropをクリックした時に閉じないようにする @type {boolean} */
  disableBackdrop?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  open: false,
  disableBackdrop: false,
});

const slots = useSlots();

const onClose = (isOpen?: boolean) => {
  emits("onClose", isOpen);
};

// escape, backdropクリック時に呼ばれる
const onKeyClose = (isOpen?: boolean) => {
  if (!props.disableBackdrop) onClose(isOpen);
};
</script>
<style lang="scss">
.modal-wrapper {
  position: relative;
  z-index: 50;
}

// 背景
.backdrop {
  position: fixed;
  z-index: 50;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.64);
}

.modal {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 51;

  .uc-modal__container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 1.25rem;
    pointer-events: none;
  }

  .uc-dialog,
  .uc-modal__surface {
    max-width: 100%;
  }

  .uc-modal__header {
    padding: 1rem 3rem;
    border-bottom: 1px solid $colors-neutral-divider;
  }

  .uc-modal__header-title {
    font-size: 0.875rem;
    color: $colors-neutral-text-secondary;
    text-align: center;
  }

  .uc-modal__close-button {
    width: 1.75rem;
    height: 1.75rem;
    top: 0.75rem;
  }

  .uc-modal__content {
    padding: 0;
    background-color: $colors-neutral-alt-background;
  }

  .uc-modal__actions {
    padding: 1rem;
    border-top: 1px solid $colors-neutral-divider;
  }
}

.modal__container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 1.25rem;
  pointer-events: none;

  > * {
    pointer-events: initial;
  }
}
</style>
