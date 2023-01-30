<template>
  <h2>求人詳細</h2>

  <section v-if="job">
    <h3>{{ job.title }}</h3>
    <div>
      <h4>概要</h4>
      <p>{{ job.description }}</p>
      <h4>カバー</h4>
      <p><img :src="job.cover" alt=""></p>
      <h4>作成日</h4>
      <p>{{ job.createdAt }}</p>
    </div>
  </section>
</template>

<script lang="ts">
import { ref, reactive } from 'vue'

export default {
  async setup() {
    const route = useRoute();
    const id = route.params.id
    const { data: job } = await useFetch(`https://62fe320941165d66bfbabe7e.mockapi.io/apt/v1/jobs/${id}`)

    return {
      job
    }
  }
}
</script>

<style>

</style>