<template>
  <h1>コンポーネント一覧</h1>

  <div class="d-flex">
    <Button
      @bottonClick="onBottonClick"
      class="base"
      text="Button1"
    />
    <Button
      @bottonClick="onBottonClick"
      class="base"
      text="Button2"
      disabled
    />
  </div>
</template>

<script setup lang="ts">
const onBottonClick = (event: Event) => {
  alert('ボタンクリック')
}

const test = ref(false);
</script>

<style lang="scss">
</style>