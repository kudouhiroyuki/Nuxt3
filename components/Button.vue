<template>
  <v-btn
    @click="onClick"
    :class="class"
    :disabled="disabled"
  >
    {{ text }}
  </v-btn>
</template>

<script setup lang="ts">
interface Props extends Partial<HTMLButtonElement> {
  class?: "base";
  text: string;
  disabled?: boolean;
}

interface Emits {
  (e: 'bottonClick', event: Event): void
}

const props = withDefaults(defineProps<Props>(), {
  class: "base",
  text: "",
  disabled: false,
});

const emits = defineEmits<Emits>()

const onClick = (event: Event) => {
  emits("bottonClick", event);
}
</script>

<style lang="scss">
button {
  &.base {
    color: #FFF;
    background-color:  #0069D9;
  }
}
</style>