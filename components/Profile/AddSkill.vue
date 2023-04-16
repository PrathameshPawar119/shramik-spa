<template>
  <div>
    <v-btn icon @click.stop="dialogBox = true" color="purple-darken-4" style="display: inline-block; position: absolute; right: 2rem; top: 1rem; ">
        <v-icon icon="$plus"></v-icon>
    </v-btn>
    <v-dialog v-model="dialogBox" :persistent="skills === null ? true : false" max-width="400">
        <section v-if="pending">
            <loading message="Loadng skills..." />
        </section>
        <section>
            <v-card class="card">
                <v-card-title>Select skill from below box</v-card-title>
                <v-autocomplete v-model="formData.skill" :items="skills" item-title="name" item-value="id" label="Select skill"
                class="ma-4"></v-autocomplete>
                <v-card-actions>
                    <v-btn variant="tonal" @click="addSkill">Submit</v-btn>
                    <v-btn id="closedialogbtn" @click="dialogBox= false">
                        Close
                    </v-btn>
                </v-card-actions>
            </v-card>
        </section>
    </v-dialog>
  </div>
</template>

<script setup>

const dialogBox = ref(false);
const {$useFetchApi:useFetchApi} = useNuxtApp();
const route = useRoute();
const reactiveQuery = computed(()=> route.query);
const skills = ref(null);
const { $auth: auth } = useNuxtApp();
const emit = defineEmits("getSkill");
const formData = reactive({
    skill:null
});

const { pending, data, error, refresh } = await useFetchApi("skills", {
    lazy:true,
})

watch(data, ()=>{
    skills.value = data.value.data;
})

async function addSkill(){
        document.getElementById("closedialogbtn").click();
    const { pending, data, error, refresh } = await useFetchApi("/customer/skillaction", {
        method:"POST",
        body:{
            customers_id: auth.value.user.data.user.id,
            skills_id:formData.skill
        },
        lazy:true,
        query:reactiveQuery
    })
    if(data.value){
        emit("getSkill", data.value.data);
    }

}
watch( reactiveQuery, ()=>{
    refresh();
})



</script>