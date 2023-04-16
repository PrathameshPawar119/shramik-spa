<template>
    <v-container>
        <section v-if="formData.submitting">
            <loading message="hold on, creating your dream true..." />
        </section>
        <section v-else>
            <section v-if="formData.submitted" class="text-center">
                <h2>Congratulations !, company created successfully.</h2>
                <h3>Please Check your registered email - {{ formData?.email }} for company dashboard link.</h3>
                <div class="d-flex justify-center">
                    <NuxtLink to="/" >
                        <v-btn color="primary" class="p-2 ma-2 mb-4" >Back to Home</v-btn>
                    </NuxtLink>
                    <NuxtLink to="/account">
                        <v-btn color="primary" class="p-2 ma-2 mb-4" >Updated Profile</v-btn>
                    </NuxtLink>
                </div>
            </section>
            <section v-else>
                <h2 class="text-center my-2">Create a New Company</h2>
                <div class="SignupComponent">
                    <v-text-field
                        v-model="formData.name"
                        label="Company Name*"
                        placeholder="Enter Company name"
                        type="text"
                        name="name"
                        variant="outlined"
                        required
                        color="primary"
                        class="mx-4"
                    ></v-text-field>
                    <v-text-field
                        v-model="formData.title"
                        label="Company Title*"
                        placeholder="Enter Company Title"
                        type="text"
                        name="title"
                        variant="outlined"
                        required
                        color="primary"
                        class="mx-4"
                    ></v-text-field>
                    <v-text-field
                        v-model="formData.email"
                        label="Company Email*"
                        placeholder="Enter Company Email"
                        type="email"
                        name="email"
                        variant="outlined"
                        required
                        color="primary"
                        class="mx-4"
                    ></v-text-field>
                    <v-textarea
                        v-model="formData.about"
                        label="About*"
                        placeholder="About"
                        type="text"
                        name="about"
                        hide-spin-buttons
                        variant="outlined"
                        color="primary"
                        class="mx-4"
                    ></v-textarea>
                    <v-row class="mx-3">
                        <v-col>
                            <v-autocomplete v-model="formData.main_city" :items="cities.getCities" item-title="name" item-value="url" label="Main City*" color="primary"
                             outlined prepend-inner-icon="$location"></v-autocomplete>
                        </v-col>
                        <v-col>
                            <v-autocomplete v-model="formData.industry_type" :items="IndustryTypes" item-title="name" item-value="url" label="Industry Type*" color="primary"
                                 outlined prepend-inner-icon="$location"></v-autocomplete>
                        </v-col>
                    </v-row>
                    <v-row class="mx-3">
                        <v-autocomplete v-model="formData.company_size" :items="CompanySize" item-title="name" item-value="url" label="Company Size*" color="primary"
                        class="mx-2"  outlined prepend-inner-icon="$location"></v-autocomplete>
                        <v-text-field
                            v-model="formData.founded"
                            label="Founded at*"
                            placeholder="Founded at"
                            type="date"
                            name="founded"
                            class="mx-2"
                            variant="outlined"
                            color="primary"
                            min="1900-01-01"
                            max="2023-12-31"
                            id="foundedDatePicker"
                        ></v-text-field>
                    </v-row>
        
        
                    <div class="d-flex justify-center">
                        <v-btn color="primary" class="p-2 ma-2 mb-4" @click.prevent="createCompany">Create</v-btn>
                    </div>
                </div>
            </section>
        </section>
    </v-container>
    
</template>
<script setup>
import { useVuelidate } from "@vuelidate/core";
import { required, helpers, minLength, integer, email, url } from "@vuelidate/validators";
import { useCitiesStore } from "@/stores/cities";
import { storeToRefs } from "pinia";

const { $auth: auth } = useNuxtApp();
const cities = useCitiesStore();
const { $useFetchApi: useFetchApi} = useNuxtApp();
const user = computed(() => {
    return auth.value.user.data.user;
});

const formData = reactive({
    name:null,
    title:null,
    about:null,
    email:null,
    website:null,
    industry_type:null,
    main_city:null,
    company_size:null,
    founded:null,
    submitted:false,
    submitting:false
}); 

const IndustryTypes = ref([
    'Painting Contractor',
    'Plumbing Contractor',
    'Carpentry Contractor',
    'Driving Services',
    'Electrician Contractor',
    'Interior Contractor',
    'Construction Contractor'
]);

const CompanySize = ref([
    'Small',
    'Medium',
    'Large',
    'Enterprises',
    'MNC'
]);
// definePageMeta({
//     middleware: ["guest"]
// })

useHead({
    title: "New Company"
})

//setting date picker max date to today.
// const foundedDatePicker = document.getElementById("foundedDatePicker");
// foundedDatePicker.max = new Date().toLocaleDateString('fr-ca')

const rules = computed(() => {
    const localRules = {
        name: {
            required: helpers.withMessage("Name is required", required),
            minLength: minLength(4),
        },
        title: {
            required: helpers.withMessage("Title is required", required),
            minLength: minLength(4),
        },
        email: {
            required: helpers.withMessage("Email is required", email),
        },
        website: {
            required: helpers.withMessage("Website is required", url),
        },
        industry_type: {
            required: helpers.withMessage("Industry type is required", required),
        },
        main_city: {
            required: helpers.withMessage("Main City is required", required),
        },
        company_size: {
            required: helpers.withMessage("Company size is required", required),
        },
        founded: {
            required: helpers.withMessage("Founded date is required", required),
        }
    };

    return localRules;
});

const v$ = useVuelidate(rules, formData, { $lazy: true })

async function createCompany()
{
    const formValid = await v$.value.$validate();
    if(formValid){
        formData.submitting = true;
        const { pending, data, error, refresh } = await useFetchApi("company/createcompany", {
            method: "POST",
            body: formData,
            lazy: true
        })
        
        if(data.value)
        {
            formData.submitting = false;
            formData.submitted = true;

            user.value.hasCompany = true;
        }
        if(error.value)
        {
            console.log(error.value)
        }
    }
    else{
        console.log(formValid);
    }
}

</script>