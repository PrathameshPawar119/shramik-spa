<template>
    <div>
        <v-card height="80" @click="dialogBox = true" class="card-glass pa-3">
            <h1 class="text-button text-center pt-4">Add Experience</h1>
        </v-card>
        <v-dialog v-model="dialogBox" :persistent="skills === null ? true : false" max-width="800">
            <section v-if="pending">
                <loading message="Loadng..." />
            </section>
            <section v-else>
                <form @submit.prevent="submitForm">
                    <v-card class="card">
                        <v-card-title>Create New Experience</v-card-title>
                        <v-text-field
                            v-model="formData.title"
                            label="Title"
                            placeholder="Enter post title"
                            name="title"
                            type="text"
                            hide-spin-buttons
                            variant="outlined"
                            required
                            color="success"
                            class="mx-4"
                            ></v-text-field>
                        <v-textarea
                            v-model="formData.content"
                            label="Content"
                            placeholder="Enter Experience summary"
                            name="content"
                            type="text"
                            hide-spin-buttons
                            variant="outlined"
                            required
                            color="success"
                            class="mx-4"
                            ></v-textarea>
                            <v-row class="mx-1">
                                <v-file-input
                                    class="mx-4"
                                    accept="image/png, image/jpeg, image/bmp, image/jpg, image/gif, image/svg"
                                    placeholder="Select Image"
                                    label="Image"
                                    prepend-icon="$camera"
                                    name="image"
                                    ref="fileInput"
                                    @change="onFileChange"
                                ></v-file-input>
                                <v-autocomplete v-model="formData.city" :items="cities.getCities" item-title="name" item-value="url" label="City"
                                class="ma-4" outlined prepend-inner-icon="$location"></v-autocomplete>
                            </v-row>
                            <v-col cols="12">
                                <section v-if="allTags.length > 0">
                                    <v-col cols="12">
                                        <v-combobox
                                        v-model="formData.tags"
                                        :items="allTags"
                                        label="Select tags"
                                        multiple
                                        chips
                                        ></v-combobox>
                                    </v-col>
                                </section>
                                <section v-else>
                                    <loading message="loading tags..." />
                                </section>
                        </v-col>
                        <v-card-actions>
                            <v-btn
                            class="me-4"
                            type="submit"
                            variant="tonal"
                            >
                            submit
                        </v-btn>
                        <v-btn @click="handleReset">
                            clear
                        </v-btn>
                        <v-btn id="closedialogbtn" @click="dialogBox= false">
                            Close
                        </v-btn>
                        
                    </v-card-actions>
                </v-card>
                </form>
            </section>
        </v-dialog>
    </div>
</template>

<script setup>
import { useCitiesStore } from "@/stores/cities";

const citySelect = ref(false);
const cities = useCitiesStore();
const dialogBox = ref(false);
const {$useFetchApi:useFetchApi} = useNuxtApp();
const route = useRoute();
const reactiveQuery = computed(() => route.query);

const formData = reactive({
    title:null,
    content: null,
    tags:[],
    image:null,
    city: null,
    imgPreview:null
})

const onFileChange = (e)=>{
    formData.image = e.target.files[0];
    
    let reader = new FileReader();
    reader.addEventListener('load', function(){
        formData.imgPreview = formData.image;
    })

}

async function submitForm()
{
    document.getElementById("closedialogbtn").click();
    const reqData = new FormData();
    reqData.append('title', formData.title);
    reqData.append('content', formData.content);
    reqData.append('image', formData.image);
    reqData.append('tags',( formData.tags).length > 0 ? formData.tags : ['new']);
    reqData.append('city', formData.city);

    try {
        const {pending, data, error, refresh} = await useFetchApi('customer/create-exp', {
            method: "POST",
            header:{
                'Content-Type':'multipart/form-data'
            },
            body:reqData
        })
    } catch (error) {
        console.log(error);
    }

}

async function handleReset()
{
    formData.title = null;
    formData.content = null;
    formData.image = null;
    formData.tags = null;
    formData.city = null;
}

const {pending, data, error, refresh} = await useFetchApi('tags/getall', {
    query: reactiveQuery
});


const allTags = computed(()=>{
    return data.value.data.map((obj)=> obj.name);
})

watch(reactiveQuery, ()=>{
    refresh();
})



</script>