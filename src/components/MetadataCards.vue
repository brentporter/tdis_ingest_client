<template>

        <v-row class="ml-2 mr-2" no-gutters>
            <!-- Agriculture -->
            <v-card
                    class="mx-auto mb-2 mt-2"
                    max-width="250"
                    min-width="250"
                    max-height="300"
                    elevation="3"
            >

                <!--<v-img src="https://cdn.vuetifyjs.com/images/cards/forest-art.jpg"></v-img>-->

                <v-row
                        class="ma-2 group"
                        color="red"
                        dark
                >
                    <v-col>
                        <v-icon
                                dark
                                center
                                color="light-blue lighten-4"
                        >
                            fas fa-folder-open
                        </v-icon>
                    </v-col>
                </v-row>

                <v-card-text>
                    <h2 class="text-h6 primary--text">
                        Datasets
                    </h2>
                    project based groupings of physical objects

                </v-card-text>

                <v-card-title>
                    <v-btn
                            color="light-blue lighten-4"
                            small
                            @click="toggleDatasetForm"
                    >
                        Create
                    </v-btn>
                    <!--<v-btn
                            class="ml-2"
                            small
                            color="green lighten-1"
                    >
                        Discuss
                    </v-btn>-->
                </v-card-title>

                <!--                        <v-fade-transition>
                                            <v-overlay
                                                    v-if="hover"
                                                    absolute
                                                    color="#036358"
                                            >
                                                <v-btn>Select Agriculture Track</v-btn>
                                            </v-overlay>
                                        </v-fade-transition>-->
            </v-card>
            <!-- Drought-->
            <v-card
                    class="mx-auto mb-2 mt-2 "
                    max-width="250"
                    min-width="250"
                    max-height="300"
                    elevation="3"
            >
                <v-row
                        class="ma-2 group"
                        color="red"
                        dark
                ><v-col>
                    <v-icon
                            dark
                            center
                            color="light-green lighten-1"
                    >
                        fas fa-globe-americas
                    </v-icon>
                </v-col>
                </v-row>
                <v-card-text>
                    <h2 class="text-h6 primary--text">
                        Geospatial Data
                    </h2>
                    Vector, Raster and other Spatial data
                </v-card-text>

                <v-card-title>
                    <v-btn
                            color="light-green lighten-1"
                            small
                            @click="toggleGeospatialForm"
                    >
                        Create
                    </v-btn>
                    <!-- <v-btn
                             class="ml-2"
                             small
                             color="orange lighten-3"
                     >
                         Discuss
                     </v-btn>-->
                </v-card-title>

            </v-card>
            <!-- Hydrology-->
            <v-card
                    class="mx-auto mb-2 mt-2 "
                    max-width="250"
                    min-width="250"
                    max-height="300"
                    elevation="3"
            >
                <v-row
                        class="ma-2 group"
                        color="red"
                        dark
                ><v-col>
                    <v-icon
                            dark
                            center
                            color="brown lighten-2"
                    >
                        fas fa-file-alt
                    </v-icon>
                </v-col>
                </v-row>

                <v-card-text>
                    <h2 class="text-h6 primary--text">
                        Data Files
                    </h2>
                    Reports, Spreadsheets, Documents et al.
                </v-card-text>

                <v-card-title>
                    <v-btn
                            color="brown lighten-2"
                            small

                    >
                        Create
                    </v-btn>
                    <!-- <v-btn
                             class="ml-2"
                             small
                             color="light-blue accent-1"
                     >
                         Discuss
                     </v-btn>-->
                </v-card-title>
            </v-card>
        </v-row>
</template>

<script>
    export default {
        name: "MetadataCards",
        data: () => ({
            overlay: false,
        }),
        methods:{
            toggleDatasetForm: function(){
                //console.log(this);
                this.$eventHub.$emit('toggleShowDatasetForm');
            },
            toggleGeospatialForm: function(){
                this.$eventHub.$emit('toggleShowGeospatialForm');
            },
            toggleJaxaTacks: function(){
                //this.$eventHub.$emit('toggleJaxa','true');
            }
        }
    }
</script>

<style scoped>

    .group {
        display: flex;
        flex: 1;
        justify-content: space-around;
        color: red;
    }
    .v-icon {
        font-size: 100px !important;
    }

</style>