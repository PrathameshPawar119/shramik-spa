<template>
      <div>
          <v-col cols="12" class="my-2" v-for="(job, i) in jobs" :key="i" >
            <v-card
              color="purple-darken-4">
              <div class="d-flex flex-no-wrap justify-start">
                  <v-avatar
                    class="ma-1"
                    size="150"
                    rounded="0"
                  >
                    <v-img style="object-fit: cover;" src="https://cdn.vuetifyjs.com/images/cards/halcyon.png"></v-img>
                  </v-avatar>
                <div class="flex">
                  <v-card-title class="text-h5" style="white-space: normal;">
                    {{job.title}}
                  </v-card-title>

                  <v-card-subtitle style="white-space: normal;">
                    {{job.description}}
                    <v-chip-group>
                      <v-chip v-for="(item, i) in job.tags" :key="i">{{ item }}</v-chip>
                      <v-chip prepend-icon="$location">{{ job.location }}</v-chip>
                    </v-chip-group>
                  </v-card-subtitle>

                  <v-card-actions>
                    <nuxt-link :to="job.id+ '/workplace'">
                      <v-btn
                        class="ms-2"
                        variant="outlined"
                        size="small"
                      >
                        View
                      </v-btn>
                    </nuxt-link>
                    <v-chip-group class="mx-4">
                      <v-chip v-for="(item, i) in job.skills" :key="i"><b>{{ item }}</b> </v-chip>
                    </v-chip-group>
                  </v-card-actions>
                </div>
              </div>
            </v-card>
          </v-col>
      </div>

</template>

<script setup>
const props = defineProps({
    jobs:{
        type:Array,
        required:true
    }
})
</script>

<style scoped>

</style>
