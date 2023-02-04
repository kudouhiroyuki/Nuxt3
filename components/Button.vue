<template>
  <v-btn
    @click="onClick"
    :disabled="disabled"
    :class="classMethod"
  >
    {{ text }}
  </v-btn>
</template>

<script setup lang="ts">
interface Props extends Partial<HTMLButtonElement> {
  disabled?: boolean;
  classMethod?: "base";
  text: string;
}

interface Emits {
  (e: 'bottonClick', event: Event): void
}

const props = withDefaults(defineProps<Props>(), {
  disabled: false,
  classMethod: "base",
  text: "",
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