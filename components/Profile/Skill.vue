<template>
    <v-chip class="ma-2 pa-4">
        <h3 @click="viewSkill">{{ skill.name }}</h3>
        <div class="ml-3" :id="skill.id" @click="removeSkill(skill.id)" style="cursor: pointer;">
            <v-icon icon="$cross"></v-icon>
        </div>
    </v-chip>
</template>
<script setup>
const { $useFetchApi: useFetchApi } = useNuxtApp();
const { $auth: auth } = useNuxtApp();
const props=  defineProps({
    skill:{
        type:Object,
        required: true
    }
})
const user = computed(()=>{
    return auth.value.user.data.user;
})

async function removeSkill(e) {
    // console.log(user.value.id, e);
    const { data } = await useFetchApi('customer/skillaction', {
        type: "DELETE",
        lazy: true,
        body: {
            customers_id: user.value.id,
            skills_id: e
        }
    });
}

async function viewSkill(){

}


</script>
