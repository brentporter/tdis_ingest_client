<template>
    <v-container fluid>
        <v-data-iterator
                :items="items"
                :items-per-page.sync="itemsPerPage"
                :page.sync="page"
                :search="search"
                :sort-by="sortBy.toLowerCase()"
                :sort-desc="sortDesc"
                hide-default-footer
        >
            <template v-slot:header>
                <v-toolbar
                        color="light-blue lighten-4"
                        class="mb-1"
                >
                    <v-text-field
                            v-model="search"
                            clearable
                            flat
                            solo-inverted
                            hide-details
                            prepend-inner-icon="mdi-magnify"
                            label="Search"
                    ></v-text-field>
                    <template v-if="$vuetify.breakpoint.mdAndUp">
                        <v-spacer></v-spacer>
                        <v-select
                                v-model="sortBy"
                                flat
                                solo-inverted
                                hide-details
                                :items="keys"
                                prepend-inner-icon="mdi-magnify"
                                label="Sort by"
                        ></v-select>
                        <v-spacer></v-spacer>
                        <v-btn-toggle
                                v-model="sortDesc"
                                mandatory
                        >
                            <v-btn
                                    large
                                    depressed
                                    color="blue"
                                    :value="false"
                            >
                                <v-icon>mdi-arrow-up</v-icon>
                            </v-btn>
                            <v-btn
                                    large
                                    depressed
                                    color="blue"
                                    :value="true"
                            >
                                <v-icon>mdi-arrow-down</v-icon>
                            </v-btn>
                        </v-btn-toggle>
                    </template>
                </v-toolbar>
            </template>

            <template v-slot:default="props">
                <v-row>
                    <v-col
                            v-for="item in props.items"
                            :key="item.name"
                            cols="12"
                            sm="6"
                            md="4"
                            lg="3"
                    >
                        <v-card>
                            <v-card-title class="subheading font-weight-bold">
                                {{ item.name }}
                            </v-card-title>

                            <v-divider></v-divider>

                            <v-list dense>
                                <v-list-item
                                        v-for="(key, index) in filteredKeys"
                                        :key="index"
                                >
                                    <v-list-item-content :class="{ 'blue--text': sortBy === key }">
                                        {{ key }}:
                                    </v-list-item-content>
                                    <v-list-item-content
                                            class="align-end"
                                            :class="{ 'blue--text': sortBy === key }"
                                    >
                                        {{ item[key.toLowerCase()] }}
                                    </v-list-item-content>
                                </v-list-item>
                            </v-list>
                        </v-card>
                    </v-col>
                </v-row>
            </template>

            <template v-slot:footer>
                <v-row
                        class="mt-2"
                        align="center"
                        justify="center"
                >
                    <span class="grey--text">Items per page</span>
                    <v-menu offset-y>
                        <template v-slot:activator="{ on, attrs }">
                            <v-btn
                                    text
                                    color="light-blue lighten-4"
                                    class="ml-2"
                                    v-bind="attrs"
                                    v-on="on"
                            >
                                {{ itemsPerPage }}
                                <v-icon>mdi-chevron-down</v-icon>
                            </v-btn>
                        </template>
                        <v-list>
                            <v-list-item
                                    v-for="(number, index) in itemsPerPageArray"
                                    :key="index"
                                    @click="updateItemsPerPage(number)"
                            >
                                <v-list-item-title>{{ number }}</v-list-item-title>
                            </v-list-item>
                        </v-list>
                    </v-menu>

                    <v-spacer></v-spacer>

                    <span
                            class="mr-4
            grey--text"
                    >
            Page {{ page }} of {{ numberOfPages }}
          </span>
                    <v-btn
                            fab
                            color="light-blue lighten-4"
                            class="mr-1"
                            @click="formerPage"
                    >
                        <v-icon>mdi-chevron-left</v-icon>
                    </v-btn>
                    <v-btn
                            fab
                            color="light-blue lighten-4"
                            class="ml-1"
                            @click="nextPage"
                    >
                        <v-icon>mdi-chevron-right</v-icon>
                    </v-btn>
                </v-row>
            </template>
        </v-data-iterator>
    </v-container>
</template>

<script>
    export default {
        name: "DataCatalogDirectory",
        data () {
            return {
                itemsPerPageArray: [4, 8, 12],
                search: '',
                filter: {},
                sortDesc: false,
                page: 1,
                itemsPerPage: 4,
                sortBy: 'name',
                keys: [
                    'Name',
                    'Date',
                    'Model_Ready',
                    'Map_Ready',
                    'Stored',
                    'Metadata',
                ],
                items: [
                    {
                        name: 'tidal_gauges',
                        date: '2020-01-10',
                        model_ready: 'no',
                        map_ready: 'yes',
                        stored: 'local',
                        metadata: 'link',
                    },
                    {
                        name: 'lidar',
                        date: '2021-06-15',
                        model_ready: 'yes',
                        map_ready: 'yes',
                        stored: 'remote',
                        metadata: 'link',
                    },
                    {
                        name: 'soil_moisture_se_tx',
                        date: '2019-09-30',
                        model_ready: 'yes',
                        map_ready: 'no',
                        stored: 'local',
                        metadata: 'link',
                    },
                    {
                        name: 'tx_hwys',
                        date: '2021-08-15',
                        model_ready: 'no',
                        map_ready: 'yes',
                        stored: 'remote',
                        metadata: 'link',
                    },
                    {
                        name: 'hurricane_harvey_high_water',
                        date: '2017-10-31',
                        model_ready: 'yes',
                        map_ready: 'yes',
                        stored: 'local',
                        metadata: 'link',
                    },
                    {
                        name: 'individual_assistance_2017',
                        date: '2018-12-31',
                        model_ready: 'no',
                        map_ready: 'yes',
                        stored: 'remote',
                        metadata: 'link',
                    },
                    {
                        name: 'zip_codes_2021',
                        date: '2021-06-01',
                        model_ready: 'yes',
                        map_ready: 'yes',
                        stored: 'local',
                        metadata: 'link',
                    },
                    {
                        name: 'county_boundaries',
                        date: '2021-01-30',
                        model_ready: 'yes',
                        map_ready: 'yes',
                        stored: 'local',
                        metadata: 'link',
                    },
                    {
                        name: 'city_boundaries',
                        date: '2021-01-30',
                        model_ready: 'yes',
                        map_ready: 'yes',
                        stored: 'remote',
                        metadata: 'link',
                    },
                    {
                        name: 'fema_flood_plains',
                        date: '2019-04-15',
                        model_ready: 'no',
                        map_ready: 'yes',
                        stored: 'local',
                        metadata: 'link',
                    },
                ],
            }
        },
        computed: {
            numberOfPages () {
                return Math.ceil(this.items.length / this.itemsPerPage)
            },
            filteredKeys () {
                return this.keys.filter(key => key !== 'Name')
            },
        },
        methods: {
            nextPage () {
                if (this.page + 1 <= this.numberOfPages) this.page += 1
            },
            formerPage () {
                if (this.page - 1 >= 1) this.page -= 1
            },
            updateItemsPerPage (number) {
                this.itemsPerPage = number
            },
        },
    }
</script>

<style scoped>

</style>