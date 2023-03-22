<template>
    <div>
        <section v-if="pending">
            <h1 class="text-center">My Profile</h1>
            <Loading message="Loading user..." />
        </section>
        <section v-else-if="error != null">
                {{ error }}
        </section>
        <section v-else>
            {{ user }}
            {{ skills }}
            <v-container class="card-glass" style="border-radius: 8px;">
                <v-row
                class="ma-4"
                style="display: flex; align-items: center; justify-content: center"
                >
                    <v-avatar color="purple-darken-4" :size="100">
                        <v-icon icon="$accountCircle" :size="100"></v-icon>
                    </v-avatar>
                </v-row>
                <v-row cols="6" style="display: flex; flex-direction: row; align-items: center; justify-content: center;" >
                    <h1 class="mx-2 px-2">{{ user?.name }}</h1>
                        <v-icon class="primary--text" icon="$pencil"></v-icon>
                </v-row>
                <v-row cols="6" style="display: flex; flex-direction: row; align-items: center; justify-content: center;" >
                    <h1 class="mx-2 px-2">{{ user?.email }}</h1>
                        <v-icon class="primary--text" icon="$pencil"></v-icon>
                </v-row>
                <v-row cols="6" style="display: flex; flex-direction: row; align-items: center; justify-content: center;" >
                    <h1 class="mx-2 px-2">{{ user?.contact }}</h1>
                        <v-icon class="primary--text" icon="$pencil"></v-icon>
                </v-row>
                <v-row justify="center" class="pa-2">
                    <v-col cols="12" class="rounded-lg pa-4 mt-2 text-center">
                        <v-card class="mx-auto rounded-lg" elevation="4" variant="outlined">
                        <v-card-title class="text-start text-h5 ma-2">Experience</v-card-title>
                        <div class="ma-4 rounded-xl py-3">
                            
                        </div>
                        </v-card>
                    </v-col>
                </v-row>
                <v-row justify="center" class="pa-2">
                    <v-col cols="12" class="rounded-lg pa-4 mt-2 text-center">
                        <v-card class="mx-auto rounded-lg" elevation="4" variant="outlined">
                            <LazyProfileAddSkill />
                            <v-card-title class="text-start text-h5 ma-2">Skills</v-card-title>
                            <div class="ma-4 rounded-xl py-3">
                                <v-row>
                                    <div
                                        v-for="(skill, i) in skills"
                                        :key="i"
                                        cols="12"
                                        sm="4"
                                    >
                                        <v-chip class="ma-2 pa-4">
                                            <h3>{{ skill.name }}</h3>
                                            <div class="ml-3" :id="skill.id" @click="removeSkill" >
                                                <v-icon icon="$cross"></v-icon>
                                            </div>
                                        </v-chip>
                                    </div>
                                </v-row>
                            </div>
                        </v-card>
                    </v-col>
                </v-row>
            </v-container>
        </section>
    </div>
</template>

<script setup>

const route = useRoute();
const { $useFetchApi:useFetchApi } = useNuxtApp();
const {$auth:auth} = useNuxtApp();
const reactiveQuery = computed(() => route.query);


definePageMeta({
    middleware:['auth']
});

const skills = ref([]);
// const experiences = ref(null);

useHead({
    title:"Profile | Shramik"
});

const user = computed(() => {
  return auth.value.user.data.user;
});

const { pending, data, error, refresh } = await useFetchApi('customer/profile', {
    lazy:true,
    query:reactiveQuery
});

watch(data, ()=>{
    skills.value = data.value.data.skills
    // experiences.value = data.value.data.experiences
});
watch(reactiveQuery, ()=>{
    refresh();
})


async function removeSkill(e){
    console.log(e.target);
    const { pending, data, error, refresh } = await useFetchApi('customer/skillaction', {
        type:"DELETE",
        lazy:true,
        body:{
            customers_id:user.value.id,
            skills_id:e
        }
    });
}



</script>
