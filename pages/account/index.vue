<template>
  <v-container class="pa-0">
    <v-col cols="12" lg="8" class="mx-auto">
      <h1>Your Account</h1>
      <v-divider class="mb-2"></v-divider>
      <h3>Hi, {{ user.name }}</h3>
      <!-- {{ user }} -->
      <v-row no-gutters>
        <v-col v-for="(account, i) in accountItems" :key="i" class="pa-1">
          <v-card height="100" :to="account.to" class="card-glass">
            <h1 class=" text-center mt-6" style="font-size: xx-large;">{{ account.title }}</h1>
          </v-card>
        </v-col>
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

      <v-app-bar fixed bottom>
        <v-btn block class="primary w-25" @click="logout">logout</v-btn></v-app-bar
      >
    </v-col>
  </v-container>
</template>

<script setup>
const { $auth: auth } = useNuxtApp();
const { $sanctumAuth } = useNuxtApp();

definePageMeta({
  middleware: ["auth"],
});

useHead({
  title: "Account",
});

const user = computed(() => {
  return auth.value.user.data.user;
});

const accountItems = ref([
  {
    title: "Profile",
    to: "/account/profile",
  },
  {
    title: "Posts",
    to: "/posts",
  },
]);

async function logout() {
  await $sanctumAuth
    .logout(logout)
    .then((result) => {
      console.log(result);
    })
    .catch((error) => {
      console.log(error);
    });
}
</script>

<style></style>
