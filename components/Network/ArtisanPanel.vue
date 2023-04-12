<template>
    <div>
        <section v-if="pending">
            <loading message="loading artisans..." />
        </section>
        <section v-else>
            <v-row justify="center" class="pa-2">
                <v-col cols="12" class="rounded-lg pa-4 mt-2 text-center">
                    <v-card class="mx-auto rounded-lg" elevation="4" variant="outlined">
                    <v-card-title class="text-start text-h5 ma-2">Artisans</v-card-title>
                        <div class="ma-4 rounded-xl py-3">
                        <v-row justify="center" class="pa-2">
                            <v-col cols="12" md="3" lg="3" sm="6" class="rounded-lg pa-4 mt-2 text-center" v-for="(artisan, i) in artisans" :key="i">
                                <LazyNetworkArtisanCard :artisan="artisan" />
                            </v-col>
                        </v-row>
                        </div>
                    </v-card>
                </v-col>
            </v-row>
        </section>
    </div>
</template>

<script setup>
const { $useFetchApi: useFetchApi } = useNuxtApp();
const route = useRoute();
const reactiveQuery = computed(() => route.query);
const artisans = ref([]);
const artisans_meta = ref([]);

const { pending, data, error, refresh } = await useFetchApi("getcustomers", {
    lazy: true,
    query: { page: 1 }
})

watch(data, () => {
    artisans.value = data.value.data.data
    // artisans_meta.value = data.value.data.meta
})

watch(reactiveQuery, ()=>{
    refresh();
})

</script>
