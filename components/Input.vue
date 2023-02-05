<template>
  <v-text-field v-model="text" label="Another input"></v-text-field>
</template>

<script setup lang="ts">
interface Props extends Partial<HTMLInputElement> {
  modelValue?: string,
}

interface Emits {
  (e: 'inputUpdate', value: string): void
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: "",
});

const emits = defineEmits<Emits>()

const text = computed({
  get: () =>  props.modelValue,
  set: (value) => {
    emits("inputUpdate", value);
  },
});
</script>

<style lang="scss">
button {
  &.base {
    color: #FFF;
    background-color:  #0069D9;
  }
}
</style>