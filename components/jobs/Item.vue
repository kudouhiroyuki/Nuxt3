<script setup lang="ts">
interface Props {
  id: string
}

const props = withDefaults(defineProps<Props>(), {
  id: ''
})

</script>

<template>
  <li>
    <p>求人ID: {{ id }}</p>
  </li>
</template>