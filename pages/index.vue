<template>
  <h1>コンポーネント一覧</h1>
  <Button
    @bottonClick="onBottonClick"
    :text="'Button'"
  />
</template>

<script setup lang="ts">
const onBottonClick = (event: Event) => {
  alert('ボタンクリック')
}
</script>

<style lang="scss">
</style>