<template>
    <div>
        <section v-if="pending">
            <Loading message="loading job ..." />
        </section>
        <section v-else>
            {{ job }}
        </section>
    </div>
</template>
<script setup>
const route = useRoute();
const reactiveQuery = computed(() => route.query);
const { $useFetchApi: useFetchApi } = useNuxtApp();
const job = ref([]);



const { pending, data, error, refresh } = await useFetchApi(`jobs/${route.params.slug}`,{
    lazy: true,
    query: { page: 1 }
})

watch(data, () => {
    job.value = data.value.data
})

watch(reactiveQuery, () => {
    refresh();
})

</script>