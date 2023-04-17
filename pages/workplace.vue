<template>
  <v-container>
    <h1 class="text-center">WorkPlace</h1>
    <section v-if="pending">
      <loading message="Fetching jobs..." />
    </section>
    <section v-else>
      <div>
        <lazy-job-card :jobs="jobs" />
      </div>
    </section>
  </v-container>
</template>

<script setup>
const route = useRoute();
const reactiveQuery = computed(() => route.query);
const { $useFetchApi: useFetchApi } = useNuxtApp();
const jobs = ref([]);



const { pending, data, error, refresh } = await useFetchApi("jobs", {
  lazy: true,
  query: { page: 1 }
})

watch(data, () => {
  jobs.value = data.value.data.data
  // artisans_meta.value = data.value.data.meta
})

watch(reactiveQuery, () => {
  refresh();
})



</script>