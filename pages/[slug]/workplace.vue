<template>
  <section v-if="pending">
    <Loading message='fetching job ...' />
  </section>
  <section v-else>
    <v-img :src="
      job.image ? job.images : `https://images.pexels.com/photos/7787732/pexels-photo-7787732.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1`
    " :aspect-ratio="mobile ? 16 / 9 : 16 / 3" :alt="job.title" class="rounded-b-xl" cover></v-img>
    <v-col cols="12" lg="10" xl="8" class="mx-auto">
      <v-row no-gutters >
            <h1 class="text-h4 mb-2">
                {{ job.title }}
            </h1>
        <v-col cols="12" lg="5" class="pa-1" style="display: flex; flex-direction: row; justify-content: space-between;">
            <div>
                <div class="d-flex" >
                  <v-icon left icon="$calender" size="x-large" style="border-right: 0.4rem dotted #de3163; min-width: 3rem">
                  </v-icon>
      
                  <h1 class="pl-2 text-body-1">
                    {{ formatDate(job.starting_date).formattedDate }}
                  </h1>
                </div>
      
                <div class="d-flex mt-2">
                  <v-icon left icon="$location" size="x-large" style="border-right: 0.4rem dotted #de3163; min-width: 3rem">
                  </v-icon>
      
                  <h1 class="pl-2 text-body-1 font-weight-bold">
                    City - {{ job.location }}
                  </h1>
                </div>
            </div>
            <div>
                <h2 class="pl-2 text-h6 mx-2">Skills: </h2>
                <v-chip-group>
                  <v-btn rounded :aria-label="job.title" color="primary"
                    variant="tonal" v-for="(item , i) in job.skills" :key="i">
                    {{item}}
                  </v-btn>
                </v-chip-group>
            </div>
          
        </v-col>
        <v-divider class="my-2"></v-divider>
        <v-col cols="12" lg="12">
          <div class="ma-2" style="display: flex; flex-direction: row; justify-content: space-between;">
            <v-row no-gutters class="pb-4" align="center" justify-lg="end">
              <v-btn rounded to="#message" :aria-label="job.title" color="primary"
                variant="tonal">
                <v-icon icon="$message" start=""></v-icon>
                Contact Now
              </v-btn>
              <v-btn @click="share" class="ml-3" variant="tonal" color="primary" icon="$share" />
            </v-row>
            <v-row no-gutters class="pb-4" justify-lg="end">
                <h1 class="pl-2 text-h6 mx-2">Tags:</h1>
                <v-chip-group>
                    <v-chip v-for="(item, i) in job.tags" :key="i">
                        {{ item }}
                    </v-chip>
                </v-chip-group>
            </v-row>

          </div>
        </v-col>
      </v-row>
      <v-row no-gutters justify="center" class="">
        <v-col cols="12" md="12" class="pa-2">
          <h2>About</h2>
          <v-sheet color="transparent" v-html="job.description"></v-sheet>
        </v-col>
      </v-row>
      <v-row no-gutters justify="center">
        <v-col cols="12" md="5">
          
        </v-col>
      </v-row>
    </v-col>
  </section>
</template>

<script setup>
const route = useRoute();
const reactiveQuery = computed(() => route.query);
const { $useFetchApi: useFetchApi } = useNuxtApp();
const job = ref([]);

const { pending, data, error, refresh } = await useFetchApi(`jobs/${route.params.slug}`,{
    lazy: true,
    query: { page: 1 }
})

watch(data, () => {
    job.value = data.value.data
})

watch(reactiveQuery, () => {
    refresh();
})


function formatDate(raw) {
    const rawDate = new Date(raw);
    const date = rawDate.getDate();
    const month = new Intl.DateTimeFormat("en-US", {
        month: "short",
    }).format(rawDate);
    const formattedDate = new Intl.DateTimeFormat("en-US", {
        month: "short",
        weekday: "short",
        day: "numeric",
    }).format(rawDate);

    return { date, month, formattedDate };
}


async function share() {
    const shareData = {
        title: job.value.title,
        tex: job.value.description,
        url: "https://www.iaa.org.in" + route.path,
    };
    try {
        await navigator.share(shareData);
    } catch (error) {
        console.log(error);
    }
}
</script>