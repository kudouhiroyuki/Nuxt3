<template>
  <HeadlessDisclosure v-slot="{ open: _open }" :default-open="defaultOpen">
    <HeadlessDisclosureButton v-if="slots.trigger" :class="triggerClass">
      <slot name="trigger" :open="_open"></slot>
    </HeadlessDisclosureButton>
    <transition
      class="transition-all"
      enter-active-class="transition duration-300 ease-out"
      enter-from-class="transform opacity-0"
      enter-to-class="transform opacity-100"
      leave-active-class="transition duration-300 ease-out"
      leave-from-class="transform opacity-0"
      leave-to-class="transform opacity-0"
      @before-enter="beforeEnter"
      @enter="enter"
      @after-enter="afterEnter"
      @before-leave="beforeLeave"
      @leave="leave"
    >
      <HeadlessDisclosurePanel v-if="open || _open" static>
        <slot name="content"></slot>
      </HeadlessDisclosurePanel>
    </transition>
  </HeadlessDisclosure>
</template>

<script setup lang="ts">
import { useSlots } from "vue";
interface Props {
  open?: boolean;
  triggerClass?: string|object;
  defaultOpen?: boolean
};

const props = withDefaults(defineProps<Props>(), {
  open: false,
  defaultOpen: false
});

const slots = useSlots();

const beforeEnter = function (el: any) {
  el.style.height = "0";
};
const enter = function (el: any) {
  el.style.height = el.scrollHeight + "px";
};
const afterEnter = function (el: any) {
  el.style.height = "";
};
const beforeLeave = function (el: any) {
  el.style.height = el.scrollHeight + "px";
};
const leave = function (el: any) {
  el.style.height = "0";
};
</script>
