<template>
    <v-card style="height:250px;min-width:400px;max-width:400px;">
        <v-card-title>Explore the Catalog
            <v-tabs v-model="active_picker_tab">
                <v-tab>User</v-tab>
                <v-tab>Organization</v-tab>
                <v-tab>TDIS</v-tab>
            </v-tabs></v-card-title>
        <v-card-text>
            <br/>
            <span class="fa-layers fa-fw fa-3x" style="transform: scale(0.9);" v-on:click="validate">
              <i color="lightblue" class="fas fa-folder fa-3x icons"></i>
                <span class="fa-layers-text fa-inverse" data-fa-transform="shrink-8 down-3" style="margin-left:1.5rem;font-weight:900">Datasets</span>
              </span>
            <span class="fa-layers fa-fw fa-3x breathing-room" style="transform: scale(0.9);">
              <i color="burlywood" class="fas fa-folder fa-3x icons"></i>
                <span class="fa-layers-text fa-inverse" data-fa-transform="shrink-8 down-3" style="margin-left:1.3rem;font-weight:900">Models</span>
              </span>
            <span class="fa-layers fa-fw fa-3x breathing-room" style="transform: scale(0.9);">
              <i color="yellowgreen" class="fas fa-folder fa-3x icons"></i>
                <span class="fa-layers-text fa-inverse" data-fa-transform="shrink-8 down-3" style="margin-left:1.1rem;font-weight:900">Maps</span>
              </span>
            <br/><br/><br/>
            Click above for different choices - log in to see more
        </v-card-text>
        <!--<div style="max-width: 210px; max-height: 160px;width: 210px; height: 160px;background-image: url('https://agw-prim-green2.csr.utexas.edu/tdis/folder-solid.svg');background-position: center center;color:white">
          <span style="position:inherit;bottom:5px;">Datasets</span><span>18</span></div>-->
    </v-card>
</template>

<script>
    export default {
        name: "Catalog_Categories_Picker",
        data: () => ({
            active_picker_tab:2,
        }),
        methods: {
            validate() {
                this.$eventHub.$emit('toggleDataSets');
            },
        }
    }
</script>

<style>
    .breathing-room {
        margin-left:1.6em !important;
    }
</style>