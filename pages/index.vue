<template>
  <h1>コンポーネント一覧</h1>

  <div class="d-flex">
    <Button
      @bottonClick="onBottonClick"
      :text="'Button'"
      :disabled="false"
    />
    <Button
      @bottonClick="onBottonClick"
      :text="'Button'"
      :disabled="true"
    />
  </div>
</template>

<script setup lang="ts">
const onBottonClick = (event: Event) => {
  alert('ボタンクリック')
}
</script>

<style lang="scss">
</style>