<template>
  <h1>求人一覧</h1>
  <div v-for="(v, k) in jobs" :key="k">
    <JobsItem :id="v.id" />
  </div>
</template>

<script lang="ts">
interface Jobs {
  id: string
}

export default {
  async setup() {
    const { data: jobs } =  await useFetch<Jobs[]>(`https://62fe320941165d66bfbabe7e.mockapi.io/apt/v1/jobs`)
    return {
      jobs,
    }
  }
}
</script>

<style>
</style>