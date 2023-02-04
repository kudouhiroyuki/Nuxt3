<template>
  <span
    v-if="props.name"
    class="icon inline-block h-[1em] w-[1em]"
    v-bind="$attrs"
    v-html="icon"
  />
  <span v-if="props.icon" class="icon inline-block h-[1em] w-[1em]" v-bind="$attrs">
    <svg
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 24 24"
      role="img"
      aria-hidden="true"
    >
      <path :d="props.icon ?? ''" />
    </svg>
  </span>
</template>

<script setup lang="ts">
const props = defineProps<{
  /** オリジナルアイコンの名前. assets/icons/ のファイル名を指定する. 参考 https://github.com/damianstasik/vue-svg-loader/issues/180  @type {string} */
  name?: string;

  /** svgアイコン. 参考 https://github.com/Templarian/MaterialDesign-JS. https://materialdesignicons.com/  @type {string} */
  icon?: string;
}>();

// Auto-load icons
const icons = Object.fromEntries(
  Object.entries(import.meta.glob("~/assets/icons/*.svg", { as: "raw" })).map(
    ([key, value]) => {
      const filename = key.split("/").pop()!.split(".").shift();
      return [filename, value];
    }
  )
);

const icon = props.name && (await icons?.[props.name]?.());
</script>
<style>
.icon svg {
  width: 100%;
  height: 100%;
  fill: currentColor;
}
</style>
