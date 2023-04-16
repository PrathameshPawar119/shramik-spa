<template>
    <v-container>
        <section v-if="pending">
            <h1 class="text-center">My Profile</h1>
            <Loading message="Loading user..." />
        </section>
        <section v-else-if="error != null">
            {{ error }}
        </section>
        <section v-else>
            <v-container class="card-glass" style="border-radius: 8px;">
                <v-row class="ma-4" style="display: flex; align-items: center; justify-content: center">
                    <v-avatar color="purple-darken-4" :size="100">
                        <v-icon icon="$accountCircle" :size="100"></v-icon>
                    </v-avatar>
                </v-row>
                <v-row cols="6" style="display: flex; flex-direction: row; align-items: center; justify-content: center;">
                    <h1 class="mx-2 px-2">{{ user?.name }}</h1>
                    <v-icon class="primary--text" icon="$pencil"></v-icon>
                </v-row>
                <v-row cols="6" style="display: flex; flex-direction: row; align-items: center; justify-content: center;">
                    <h1 class="mx-2 px-2">{{ user?.email }}</h1>
                    <v-icon class="primary--text" icon="$pencil"></v-icon>
                </v-row>
                <v-row cols="6" style="display: flex; flex-direction: row; align-items: center; justify-content: center;">
                    <h1 class="mx-2 px-2">{{ user?.contact }}</h1>
                    <v-icon class="primary--text" icon="$pencil"></v-icon>
                </v-row>
                <v-row no-gutters class="mt-4">
                    <v-col class="pa-1">
                        <LazyProfileCreatePost />
                    </v-col>
                    <v-col class="pa-1">
                        <LazyProfileExperiences />
                    </v-col>
                    <v-col class="pa-1" v-if="user?.hasCompany == false">
                        <NuxtLink to="/newcompany">
                            <v-card height="80" class="card-glass pa-3">
                                <h1 class="text-button text-center pt-4">Create Company</h1>
                            </v-card>
                        </NuxtLink>
                    </v-col>
                    <v-col class="pa-1" v-if="user?.hasCompany == true">
                        <a href="http://localhost:8000/contractor/login" target="_blank">
                            <v-card height="80" class="card-glass pa-3">
                                <h1 class="text-button text-center pt-4">Company Dashboard</h1>
                            </v-card>
                        </a>
                    </v-col>
                </v-row>
                <v-row justify="center">
                    <ProfileExperiencesSection :id="user.id" />
                </v-row>
                <v-row justify="center" class="pa-2">
                    <v-col cols="12" class="rounded-lg pa-4 mt-2 text-center">
                        <v-card class="mx-auto rounded-lg" elevation="4" variant="outlined">
                            <LazyProfileAddSkill @getSkill="updateSKill($event)" />
                            <v-card-title class="text-start text-h5 ma-2">Skills</v-card-title>
                            <div class="ma-4 rounded-xl py-3">
                                <v-row>
                                    <div v-for="(skill, i) in skills" :key="i" cols="12" sm="4">
                                        <LazyProfileSkill :skill="skill" />
                                    </div>
                                </v-row>
                            </div>
                        </v-card>
                    </v-col>
                </v-row>
            </v-container>
        </section>
    </v-container>
</template>

<script setup>
const route = useRoute();
const { $useFetchApi: useFetchApi } = useNuxtApp();
const { $auth: auth } = useNuxtApp();
const reactiveQuery = computed(() => route.query);

definePageMeta({
    middleware: ['auth']
});

const skills = ref([]);
// const experiences = ref(null);

useHead({
    title: "Profile | Shramik"
});

const user = computed(() => {
    return auth.value.user.data.user;
});

const { pending, data, error, refresh } = await useFetchApi('customer/profile', {
    lazy: true,
    query: reactiveQuery
});

watch(data, () => {
    skills.value = data.value.data.skills
    // experiences.value = data.value.dnata.experiences
});
watch(reactiveQuery, () => {
    refresh();
})


// emits here
const updateSKill = (event) => {
    skills.value.push(event);
}

</script>
