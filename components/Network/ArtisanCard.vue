<template>
    <div>
        <v-card>
            <v-avatar @click="navigateTo(`/${artisan.id}/social`)" class="cursor-pointer" color="grey-darken-3" size="100"
                image="https://avataaars.io/?avatarStyle=Transparent&topType=ShortHairShortCurly&accessoriesType=Prescription02&hairColor=Black&facialHairType=Blank&clotheType=Hoodie&clotheColor=White&eyeType=Default&eyebrowType=DefaultNatural&mouthType=Default&skinColor=Light"></v-avatar>
            <v-card-title>{{ artisan.name }}</v-card-title>
            <v-card-text>{{ artisan.title }}</v-card-text>
            <v-card-actions>
                <v-row style="display: grid; grid-template-columns: 1fr 2fr 1fr; grid-template-rows: 1fr;">
                    <v-btn text prepend-icon="$bookmark">save</v-btn>
                    <v-btn text variant="tonal">Contact</v-btn>
                    <v-btn text prepend-icon="$share"></v-btn>
                </v-row>
            </v-card-actions>
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