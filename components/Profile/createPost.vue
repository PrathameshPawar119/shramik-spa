<template>
    <div>
        <v-card height="80" @click="dialogBox = true" class="card-glass pa-3">
            <h1 class="text-button text-center pt-4">Add Post</h1>
        </v-card>
        <v-dialog v-model="dialogBox" :persistent="skills === null ? true : false" max-width="400">
            <section v-if="pending">
                <loading message="Loadng skills..." />
            </section>
            <section>
                <v-card class="card">
                    <button class="mx-auto bg-gray hover:bg-slate-400 pa-3 " id="closeModal" @click="dialogBox = false"><v-icon icon="$close"></v-icon></button>
                    <v-card-title>Create a Post</v-card-title>
                    <!--<v-autocomplete v-model="formData.skill" :items="skills" item-title="name" item-value="url" label="Search"
                    class="ma-4"></v-autocomplete>-->
                    <v-btn variant="tonal" class="mx-auto my-1 " @click="addSkill">Submit</v-btn>
                </v-card>
            </section>
        </v-dialog>
    </div>
</template>

<script setup>
const dialogBox = ref(false);
const {$useFetchApi:useFetchApi} = useNuxtApp();
const route = useRoute();

const formData = reactive({
    title:null,
    content: null,
    tags:[],
})






</script>