<template>
    <v-row no-gutters align="center" justify="center">
        <v-col cols="12" class="d-flex justify-center mb-4">
            <v-btn color="primary" @click.stop="filterNav = !filterNav"><v-icon icon="$filter" start></v-icon>filter
            </v-btn>
        </v-col>
        {{ selected }}
        <div v-for="(filter, index) in appliedFilters" :key="index">
            {{ index }}:
            <v-chip v-for="(option, i) in filter" :key="i" class="ma-1 font-weight-black" color="primary" variant="outlined"
                label closable @click:close="removeFilter(index, option)">
                {{ option }}
            </v-chip>
        </div>

        <v-navigation-drawer v-model="filterNav" temporary fixed app location="right"
            class="ma-2 rounded card-glass visualIndex">
            <div class="ma-2">
                <v-btn color="primary" class="mr-2" @click="applyFilter">Apply</v-btn>
                <v-btn @click="reset" variant="outlined">Reset</v-btn>
            </div>

            <v-list>
                <v-list-group v-for="(filter, i) in filterList" :key="i">
                    <template v-slot:activator="{ props }">
                        <v-list-item v-bind="props">
                            {{ filter.name.charAt(0).toUpperCase() + filter.name.slice(1) }}

                            <span v-if="selected[filter.name]">{{
                                selected[filter.name].length > 0
                                ? "(" + selected[filter.name].length + ")"
                                : null
                            }}</span>
                        </v-list-item>
                    </template>

                    <v-checkbox v-for="(option, index) in filter.options" :key="index" :label="option.name"
                        :value="option.name" class="mx-4" density="compact" hide-details
                        :model-value="selected[filter.name]"
                        @update:model-value="updateFilters(filter.name, option.name)"></v-checkbox>
                </v-list-group>
                <!-- :model-value="selected[filter.name]"
        @update:model-value="updateFilters(filter.name, option.name)" -->
            </v-list>
        </v-navigation-drawer>
    </v-row>
</template>

<script setup>
const router = useRouter();
const route = useRoute();
const { $useFetchApi: useFetchApi } = useNuxtApp();
const reactiveQuery = route.query;
const props = defineProps({
    filterList: {
        type: Array,
        required: true,
    },
});
// controls navigation of filter drawer
const filterNav = ref(false);
// controls the filter checkboxes to show current selection
let selected = reactive({});
// controls the chips to show applied filters
let appliedFilters = reactive({});
const skills = ref([]);

function updateFilters(name, option) {
    if (!selected[name]) {
        selected[name] = [];
    }

    const isPresent = selected[name].findIndex((element) => element === option);
    isPresent >= 0 ? selected[name].splice(isPresent, 1) : selected[name].push(option);

    // do not delete the dynamic keys even if empty array, crucial for v-model functioning
}

function removeFilter(filter, option) {
    selected[filter] = selected[filter].filter(function (element) {
        return element !== option;
    });
    applyFilter();
}

watch(
    () => props.filterList,
    () => {
        parseQueryFilters();
    }
);

onMounted(() => {
    parseQueryFilters();
});

//fetching skills to select
const { pending, data, error, refresh } = await useFetchApi('skills', {
    lazy:true
});
watch(data, ()=>{
    skills.value = data.value.data;
    props.filterList.push({
        name: 'skills',
        options: data.value.data
    });
})

function parseQueryFilters() {
    const queryFilters = route.query;
    Object.keys(queryFilters).forEach((key) => {
        // check if route query filters exists in fetched filters array
        if (props.filterList.some((element) => element.name === key)) {
            // set filters to selected if exists
            selected[key] = queryFilters[key].split(",");
            // set applied filter chips
            appliedFilters[key] = queryFilters[key].split(",");
        }
    });
}

function applyFilter() {
    const filters = { ...selected };
    Object.keys(filters).forEach((key) => {
        if (filters[key].length > 0) {
            filters[key] = filters[key].toString();
        } else {
            delete filters[key];
        }
    });

    navigateTo({ query: filters });
    // router.replace({ query: filters });
}

function reset() {
    navigateTo(route.path);
}
</script>
