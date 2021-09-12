<template>
    <v-app>
        <v-navigation-drawer app :mini-variant.sync="mini">
            <v-list-item>
                <v-list-item-avatar>
                    <v-img src="https://moves.csr.utexas.edu/tdis_backbone_poc/views/images/tdis_filler.png"></v-img>
                </v-list-item-avatar>
                <v-list-item-content>
                    <v-list-item-title class="text-h6">
                        Navigate
                    </v-list-item-title>
                    <v-list-item-subtitle>
                        TDIS App
                    </v-list-item-subtitle>

                </v-list-item-content>
                <v-btn
                        icon
                        @click.stop="mini = !mini"
                >
                    <v-icon>mdi-chevron-left</v-icon>
                </v-btn>
            </v-list-item>

            <v-divider></v-divider>

            <v-list
                    dense
                    nav
            >
                <v-list-item
                        v-for="item in items"
                        :key="item.title"
                        link
                        @click="flipMetadata"
                >
                    <v-list-item-icon>
                        <v-icon>{{ item.icon }}</v-icon>
                    </v-list-item-icon>

                    <v-list-item-content>
                        <v-list-item-title>{{ item.title }}</v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
            </v-list>
        </v-navigation-drawer>
        <v-app-bar flat dense style="max-height: 62px;">
            <!-- <v-app-bar-nav-icon @click.stop="mini = !mini"></v-app-bar-nav-icon>-->
            <!--<v-list-item-avatar>-->
            <img src="https://moves.csr.utexas.edu/tdis_backbone_poc/views/images/tdis_filler.png" width="120px" style="margin-top: 10px;" height="60px"/>
            <!--</v-list-item-avatar>-->
            <v-toolbar-title class="ml-1">TDIS Data Ingest Client POC</v-toolbar-title>
        </v-app-bar>
        <v-main>
            <v-container fluid>
                <transition name="fade-transition">
                    <v-row v-show="showMap" class="ml-2 mb-2 pa-3 flex-row d-flex d-sm-flex d-lg-flex"  float>
                        <v-col flex class="mb-auto">
                            <v-card flex elevation="3" style="min-width:400px;max-width:400px;">
                                <!-- <v-card
                                         class="mx-auto"
                                         max-width="344"
                                         elevation="3"
                                 >
                                   <v-img
                                           src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg"
                                           height="200px"
                                   ></v-img>

                                   <v-card-title>
                                     Top western road trips
                                   </v-card-title>

                                   <v-card-subtitle>
                                     1,000 miles of wonder
                                   </v-card-subtitle>

                                   <v-card-actions>
                                     <v-btn
                                             color="orange lighten-2"
                                             text
                                     >
                                       Explore
                                     </v-btn>

                                     <v-spacer></v-spacer>

                                     <v-btn
                                             icon
                                             @click="show = !show"
                                     >
                                       <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
                                     </v-btn>
                                   </v-card-actions>

                                   <v-expand-transition>
                                     <div v-show="show">
                                       <v-divider></v-divider>

                                       <v-card-text>
                                         I'm a thing. But, like most politicians, he promised more than he could deliver. You won't have time for sleeping, soldier, not with all the bed making you'll be doing. Then we'll go with that data file! Hey, you add a one and two zeros to that or we walk! You're going to do his laundry? I've got to find a way to escape.
                                       </v-card-text>
                                     </div>
                                   </v-expand-transition>
                                 </v-card>-->
                                <Catalog_Categories_Picker/>
                            </v-card>
                        </v-col>
                        <v-col flex class="mb-auto">
                            <v-card elevation="3" style="min-width:400px;max-width:400px;">
                                <Login_TDIS/>
                                <!--<DataTaxonomy1 />-->
                            </v-card>
                        </v-col>
                    </v-row>
                </transition>
                <transition name="fade-transition" >
                    <v-row v-show="showMap2" class="ml-2 mb-2 pa-3 mr-1">
                        <!-- <v-btn
                                 color="green lighten-2"
                                 class="mr-4 mb-3"

                                 v-show="visibleMetadataFlg"
                         >
                           <v-icon class="mr-3">fas fa-home</v-icon> Home
                         </v-btn>-->
                        <v-card elevation="3" min-width="100%">
                            <v-card-title>Create and Load Metadata
                                <v-btn
                                        color="light-blue"
                                        fab
                                        small
                                        style="margin-left:10px;"
                                        @click="hideMetadata"
                                >
                                    <v-icon>mdi-close</v-icon>
                                </v-btn>
                            </v-card-title>

                            <MetadataCards/>
                            <!-- <MetadataForm />-->
                        </v-card>
                    </v-row>
                </transition>
                <transition name="fade-transition" v-on:after-enter="afterMetadataEnter">
                    <v-row v-show="showDatasetMetadata" class="ml-2 mb-2 pa-3">
                        <!-- <v-btn
                                 color="green lighten-2"
                                 class="mr-4 mb-3"

                                 v-show="displayDatasetMetadata"
                         >
                           <v-icon class="mr-3">fas fa-home</v-icon> Home
                         </v-btn>-->
                        <v-card elevation="3" min-width="100%">
                            <!--   <TaxonomyWorksheet />-->
                            <MetadataForm/>
                        </v-card>
                    </v-row>
                </transition>
                <transition name="fade-transition" v-on:after-enter="afterMetadataEnter">
                    <v-row v-show="showGeospatialMetadata" class="ml-2 mb-2 pa-3">
                        <!-- <v-btn
                                 color="green lighten-2"
                                 class="mr-4 mb-3"

                                 v-show="displayDatasetMetadata"
                         >
                           <v-icon class="mr-3">fas fa-home</v-icon> Home
                         </v-btn>-->
                        <v-card elevation="3" min-width="100%">
                            <!--   <TaxonomyWorksheet />-->
                            <GeospatialMetadataForm />
                        </v-card>
                    </v-row>
                </transition>
                <transition name="fade-transition">
                    <v-row v-show="showDataCatalog" class="ml-2 mb-2 pa-3">
                        <!-- <v-btn
                                 color="green lighten-2"
                                 class="mr-4 mb-3"

                                 v-show="displayDatasetMetadata"
                         >
                           <v-icon class="mr-3">fas fa-home</v-icon> Home
                         </v-btn>-->
                        <v-card elevation="3" min-width="100%">
                            <!--   <TaxonomyWorksheet />-->
                            <DataCatalogDirectory/>
                        </v-card>
                    </v-row>
                </transition>
            </v-container>
        </v-main>
    </v-app>
</template>
<script>
    // import HelloWorld from './components/HelloWorld';
    import Vue from 'vue';
    import Catalog_Categories_Picker from "@/components/Catalog_Categories_Picker";
    import DataCatalogDirectory from "@/components/DataCatalogDirectory";
    import GeospatialMetadataForm from "@/components/GeospatialMetadataForm";
    import Login_TDIS from "@/components/Login_TDIS";
    import MetadataCards from "@/components/MetadataCards";
    import MetadataForm from "@/components/MetadataForm";

    Vue.prototype.$eventHub = new Vue();

    export default {
        name: 'App',
        components: {
            Catalog_Categories_Picker,
            DataCatalogDirectory,
            GeospatialMetadataForm,
            Login_TDIS,
            MetadataCards,
            MetadataForm,
        },
        created(){
            this.$eventHub.$on('displayMetadataPage', this.flipMetadata);
            this.$eventHub.$on('resetCredentials', this.resetCredentials);
            this.$eventHub.$on('toggleShowDatasetForm', this.displayDatasetMetadata);
            this.$eventHub.$on('toggleShowGeospatialForm', this.displayGeospatialMetadata);
            //showDataCatalog
            this.$eventHub.$on('toggleDataSets', this.showDataCatalogFunction);
            //toggleShowDatasetForm
        },
        data: () => ({
            drawer: false,
            group: null,
            loginStatus: false,
            mini: true,
            show: false,
            showDataCatalog: false,
            showMap: true,
            showMap2: false,
            showMap3: false,
            showTable: false,
            showDatasetMetadata: false,
            showGeospatialMetadata: false,
            //toggleDataSets: false,
            visibleMetadataFlg: false,
            visibleTaxonomyFlg: false,
            items: [
                { title: 'Dashboard', icon: 'mdi-view-dashboard' },
                { title: 'Catalog', icon: 'mdi-folder' },
                { title: 'Data Upload', icon: 'mdi-image' },
                { title: 'Metadata', icon: 'mdi-alpha-m-circle' },
                { title: 'User', icon: 'mdi-account' },
                { title: 'About', icon: 'mdi-help-box' },
            ],
            right: null,

        }),
        methods:{
            afterMetadataEnter:function(){
                //console.log("I am in the after Metadata enter");
                //console.log("I am in the after Metadata enter ");
                this.$eventHub.$emit('invalidateSizeMap');
                //this.visibleMetadataFlg = true;
                //window.resize();
                //this.$eventHub.$emit('invalidateSizeMap');
            },
            displayDatasetMetadata: function(){
                this.showDatasetMetadata = true;
                this.showMap2 = false;

            },
            flipMetadata:function(){
                this.showMap2 = true;
            },
            displayGeospatialMetadata: function(){
                this.showGeospatialMetadata = true;
                this.showDatasetMetadata = false;
                this.showMap2 = false;
            },
            hideMetadata:function(){
                this.showMap2 = false;
            },
            resetCredentials:function(){
                this.showMap2 = false;
                this.showDatasetMetadata = false;
                this.showGeospatialMetadata = false;
            },
            showDataCatalogFunction: function(){
                this.showDataCatalog = !this.showDataCatalog;
            }

        },

    };
</script>
<style>
    .icons svg {
        color:#2759AE;
    }
    .svg-inline{
        color: burlywood;
    }
    .breathing-room {
        margin-left:2.1em;
    }
    .
    ::after, ::before {

        text-decoration: inherit;
        vertical-align: inherit;

    }
    element {

        height: 100vh;
        top: 0px;
        transform: translateX(0%);
        width: 240px;
        max-height: calc(100% + 0px);

    }
    .v-navigation-drawer--clipped:not(.v-navigation-drawer--temporary):not(.v-navigation-drawer--is-mobile) {

        z-index: 4;

    }
    .theme--light.v-navigation-drawer {

        background-color: #fff;

    }
    .v-navigation-drawer {
        top: 80px !important;
        height: calc(100vh - 64px) !important;
    }
    .calendar-text { margin-left:0.9em ;margin-top: .3em; color: white; font-size: 1.5rem !important;}
    /* @media screen and (max-width: 430px) {
      .row {
       margin-left: 2px !important;
       padding: 3px !important;

      }
     }*/
</style>
