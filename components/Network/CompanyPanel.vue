<template>
    <div>
        <section v-if="pending">
            <loading message="loading companies..." />
        </section>
        <section v-else>
            <v-row justify="center" class="pa-2">
                <v-col cols="12" class="rounded-lg pa-4 mt-2 text-center">
                    <v-card class="mx-auto rounded-lg" elevation="4" variant="outlined">
                        <v-card-title class="text-start text-h5 ma-2">Companies</v-card-title>
                        <div class="ma-4 rounded-xl py-3">
                            companies here
                        </div>
                    </v-card>
                </v-col>
            </v-row>
        </section>
    </div>
</template>

<script setup>
import { useCitiesStore } from "@/stores/cities";
import { storeToRefs } from "pinia";

const { $useFetchApi: useFetchApi } = useNuxtApp();
const cities = useCitiesStore();
const { defaultCity, cookieCity } = storeToRefs(cities);
const route = useRoute();
const reactiveQuery = computed(() => route.query);

const companies = ref([]);
const companies_meta = ref([]);

const { pending, data, error, refresh } = await useFetchApi("company/allcompanies", {
    method: 'POST',
    body: {
        'type': 'popular',
        'services': null
    },
    lazy: true,
    query: { page: 1 }
})

watch(data, () => {
    companies.value = data.value.data
    // companies_meta.value = data.value.data.meta
})

watch(reactiveQuery, () => {
    refresh();
})

</script>
<style scoped>
/* .headline{
  background: linear-gradient(to left, blue, violet);
} */
</style>
