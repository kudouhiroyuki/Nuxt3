<template>
  <h1>コンポーネント一覧</h1>

  <div class="d-flex mb-5">
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

  <div class="mb-5">
    <Input
      @inputUpdate="onInputUpdate('input1', $event)"
      v-model:modelValue="state.input1"
    />
    <Input
      @inputUpdate="onInputUpdate('input2', $event)"
      v-model:modelValue="state.input2"
    />
  </div>
</template>

<script setup lang="ts">
interface State {
  input1: string;
  input2: string;
  [key: string]: string;
}

const state: State = reactive({
  input1: "",
  input2: ""
})

const onBottonClick = (event: Event) => {
}

const onInputUpdate = (key: string, value: string) => {
  state[key] = value
}
</script>

<style lang="scss">
</style>