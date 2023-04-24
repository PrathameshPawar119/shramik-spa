<template>
    <div>
        <v-card>
            <v-avatar @click="navigateTo(`/${artisan.id}/social`)" class="cursor-pointer" color="grey-darken-3" style="width:4em; height:4em;"
                image="https://avataaars.io/?avatarStyle=Transparent&topType=ShortHairShortCurly&accessoriesType=Prescription02&hairColor=Black&facialHairType=Blank&clotheType=Hoodie&clotheColor=White&eyeType=Default&eyebrowType=DefaultNatural&mouthType=Default&skinColor=Light"></v-avatar>
            <v-card-title class="title">{{ artisan.name }}</v-card-title>
            <v-card-text class="mb-0">{{ artisan.title }}</v-card-text>
            <v-row class="mb-1 mx-1 mt-0" style="display: grid; grid-template-columns: 1fr 2fr 1fr; grid-template-rows: 1fr;">
                <v-btn text prepend-icon="$bookmark"></v-btn>
                <v-btn text variant="tonal">Contact</v-btn>
                <v-btn text prepend-icon="$share"></v-btn>
            </v-row>
        </v-card>
    </div>
</template>
<script setup>

const props = defineProps({
    artisan: {
        type: Object,
        required: true
    }
})
</script>

<style scoped>
.title{
    font-size:20px;
}
</style>