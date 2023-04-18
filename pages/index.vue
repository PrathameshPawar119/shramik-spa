<template>
    <v-container class="text-center">
      <section v-if="pending">
        <loading message="Loadng Posts..." />
      </section>
      <section v-else>
        <div class="displaySection">
          <div class="leftSection d-none d-sm-flex">
            <filter-nav :filter-list="filters" />
          </div>
          <div class="postsSection">
            <post-card v-for="(post, i) in posts" :key="i" :post="post" type="postcard"/>
          </div>
        </div>
      </section>
    </v-container>
</template>

<script setup>
import { useCitiesStore } from "@/stores/cities";
import { storeToRefs } from "pinia";

const {$useFetchApi:useFetchApi} = useNuxtApp();
const cities = useCitiesStore();
const { defaultCity, cookieCity } = storeToRefs(cities);
const route = useRoute();
const reactiveQuery = computed(() => route.query);
const drawer = ref(false);


useHead({
  title:"Home"
})

const filters = [
  {
    name:'type',
    options:[
      {name: 'latest'},
      {name: 'popular'}
    ]
  },
]

const posts = ref(null);
const {pending, data, error, refresh } = await useFetchApi("/", {
  lazy:true,
  query:reactiveQuery
})

// onMounted(() => { 
//   console.log(defaultCity);
//   if (defaultCity.value) {
//     navigateTo(`/${defaultCity.value}/city`);
//   }
// });

watch(data,()=>{
  posts.value = data.value.data.data
})

watch( reactiveQuery ,()=>{
  refresh();
})

</script>

<style scoped>
.postsSection{
  display: grid;
  grid-template-columns:1fr 1fr;
  grid-template-rows: 1fr 1fr;
}

@media screen and (max-width: 660px){
  .postsSection{
    display: grid;
    grid-template-columns: 1fr;
    grid-template-rows: 1fr;
  }

}

</style>