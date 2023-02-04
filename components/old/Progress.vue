<template>
  <div class="uc-progress" :class="{ 'uc-progress--indeterminate': isIndeterminate }">
    <svg
      class="uc-progress__circle"
      xmlns="http://www.w3.org/2000/svg"
      :style="{
        width: size + 'px',
        height: size + 'px',
      }"
      :viewBox="`0 0 ${size} ${size}`"
    >
      <circle
        v-if="hasBackground"
        class="uc-progress__circle-background"
        :cx="cx"
        :cy="cy"
        :r="r"
        :style="{ transform: `translate(${stroke / 2}px, ${stroke / 2}px)` }"
        :stroke-width="stroke"
        :stroke-dasharray="strokeDasharray"
      ></circle>
      <circle
        class="uc-progress__circle-indicator"
        :cx="cx"
        :cy="cy"
        :r="r"
        :style="{ transform: `translate(${stroke / 2}px, ${stroke / 2}px)` }"
        :stroke-width="stroke"
        :stroke-dasharray="strokeDasharray"
        :stroke-dashoffset="strokeDashOffset"
      ></circle>
    </svg>
  </div>
</template>

<script setup lang="ts">

// --------------------------------------------------------------------------
// props
// --------------------------------------------------------------------------
interface Props {
  /** 不確定かどうか @type {boolean} */
  isIndeterminate?: boolean;

  /** 半径 @type {number} */
  radius?: number;

  /** 進捗率(%). 0~100 の数値 @type {number} */
  value?: number;

  /** 線の幅 @type {boolean} */
  stroke?: number;

  /** 背景を持つかどうか @type {boolean} */
  hasBackground?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  isIndeterminate: false,
  radius: 24,
  value: 35,
  stroke: 4,
  hasBackground: true,
});

const size = props.radius * 2;
const cx = (size - props.stroke) / 2;
const cy = (size - props.stroke) / 2;
const r = (size - props.stroke) / 2;

const strokeDasharray = r * 2 * Math.PI;
const strokeDashOffset = (strokeDasharray * (100 - props.value)) / 100;
</script>
