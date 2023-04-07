<template>
    <div style="width: 100%;">
        <section v-if="pending">
            <h1 class="text-center">Experiences</h1>
            <Loading message="..." />
        </section>
        <section v-else-if="error != null">
                {{ error }}
        </section>
        <section v-else class="cols-12">
            <v-col cols="12" class="rounded-lg pa-4 mt-2 text-center  w-full">
                <v-card class="mx-auto rounded-lg" elevation="4" variant="outlined">
                <v-card-title class="text-start text-h5 ma-2">Experience</v-card-title>
                <div>
                   <div v-if="experiences.length <1">
                        <h4 class="text-center m-4 pb-8">No Experiences added yet.</h4>
                   </div> 
                   <div v-else>
                       <div class=" ma-4 px-4 rounded-xl py-3" style="display: flex; flex-direction: row; justify-content: space-evenly;">
                           <PostCard v-for="(item, i) in experiences" :key="i" :post="item" type="experience-card"/>
                       </div>
                   </div>
                </div>
                </v-card>
            </v-col>
        </section>
    </div>
</template>

<script setup>
const props = defineProps({
    id:{
        type:Number,
        required:true
    }
})
const route = useRoute();
const { $useFetchApi:useFetchApi } = useNuxtApp();
const reactiveQuery = computed(() => route.query);

const experiences = ref(null);

const { pending, data, error, refresh } = await useFetchApi(`customer/experiences/${props.id}`, {
    lazy:true,
    query:reactiveQuery
});

watch(data, ()=>{
    experiences.value = data.value.data
    // experiences.value = data.value.data.experiences
});
watch(reactiveQuery, ()=>{
    refresh();
})


</script>