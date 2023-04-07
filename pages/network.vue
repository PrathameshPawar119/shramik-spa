<template>
  <div>
    <section v-if="pending">
      <loading message="loading networks..." />
    </section>
    <section v-else>
      <v-container>
        <div>
          <network-artisan-panel />
        </div>
        <div>
          <network-company-panel />
        </div>
      </v-container>
    </section>
  </div>
</template>

<script setup>
import { useCitiesStore } from "@/stores/cities";
import { storeToRefs } from "pinia";

const {$useFetchApi:useFetchApi} = useNuxtApp();
const cities = useCitiesStore();
const { defaultCity, cookieCity } = storeToRefs(cities);
const route = useRoute();
const reactiveQuery = computed(() => route.query);



</script>
<style scoped>
/* .headline{
  background: linear-gradient(to left, blue, violet);
} */
</style>
