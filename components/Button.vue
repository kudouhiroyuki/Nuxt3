<template>
	<button type="button" @click="onClick">
    {{ text }}
	</button>
</template>

<script setup lang="ts">
interface Props extends Partial<HTMLButtonElement> {
  text: string;
}

interface Emits {
  (e: 'bottonClick', event: Event): void
}

const props = withDefaults(defineProps<Props>(), {
  text: "",
});

const emits = defineEmits<Emits>()

const onClick = (event: Event) => {
  emits("bottonClick", event);
}
</script>

<style lang="scss">
</style>