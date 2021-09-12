<template>

    <v-row class="ml-2 mr-2" no-gutters>
        <v-card
                class="mx-auto ml-5 mb-2 mt-2 pa-3">

            <v-card-title>Geospatial Metadata form</v-card-title>

            <v-form
                    ref="form"
                    v-model="valid"
                    lazy-validation
            >
                <v-text-field
                        v-model="title"
                        :counter="26"
                        :rules="titleRules"
                        label="Title"
                        required
                        clearable
                ></v-text-field>
                <v-textarea
                        background-color="grey lighten-2"
                        v-model="description"
                        filled
                        label="Summary Description"
                        auto-grow
                        clearable
                        required
                        hint="Insert the description of the Collection here."
                ></v-textarea>
                <v-text-field
                        v-model="purpose"
                        label="Purpose/Use"
                        :rules="purposeRules"
                        required
                        clearable
                        hint="Describe the uses and purpose of the Collection"
                ></v-text-field>
                <v-select
                        v-model="formats_selected"
                        multiple
                        chips
                        :items="format_types"
                        :rules="[v => !!v || 'Item is required']"
                        label="Format Types"
                        required
                ></v-select>
                <v-textarea
                        background-color="grey lighten-2"
                        v-model="table_of_contents"
                        name="Table of Contents"
                        filled
                        label="Table of Contents"
                        auto-grow
                        clearable
                        hint="Insert the list of datasets within the Collection here."
                ></v-textarea>
                <v-text-field
                        v-model="spatial_extent"
                        label="Spatial Extent"
                        required
                        clearable
                        hint="Insert Well Known name of Spatial Area or Coordinates Extent from Map Below"
                ></v-text-field>
                <v-card class="mb-5">
                    <v-card-title>Find Spatial Extent
                        <v-btn
                                small
                                color="primary"
                                dark
                                class="ml-3"
                                @click="getMapExtent"
                        >
                            Use this Extent
                        </v-btn>
                        <v-btn
                                small
                                color="red"
                                dark
                                class="ml-3"
                                @click="resetMap"
                        >
                            Reset
                        </v-btn>
                    </v-card-title>
                    <v-card-text>
                        <div id="mapContainer"></div>
                    </v-card-text>
                </v-card>
                <v-divider light></v-divider>
                <!-- <v-text-field
                         v-model="email"
                         :rules="emailRules"
                         label="E-mail"
                         required
                 ></v-text-field>-->
                <v-menu
                        ref="menu"
                        v-model="menu"
                        :close-on-content-click="false"
                        :return-value.sync="date"
                        transition="scale-transition"
                        offset-y
                        min-width="auto"
                >
                    <template v-slot:activator="{ on, attrs }">
                        <v-text-field
                                v-model="date"
                                label="Last Publication Date"
                                prepend-icon="far fa-calendar-alt"
                                readonly
                                v-bind="attrs"
                                v-on="on"
                                color="green lighten-1"
                        >
                        </v-text-field>
                    </template>
                    <v-date-picker
                            v-model="date"
                            scrollable
                            event-color="green lighten-1"
                            color="green lighten-1"
                            header-color="green lighten-1"
                    >
                        <v-spacer></v-spacer>
                        <v-btn
                                color="green lighten-1"
                                @click="menu = false"
                        >
                            Cancel
                        </v-btn>
                        <v-btn
                                text
                                color="green lighten-1"
                                @click="$refs.menu.save(date)"
                        >
                            OK
                        </v-btn>
                    </v-date-picker>
                </v-menu>
                <v-select
                        v-model="select_constraints"
                        chips
                        :items="constraints_use"
                        :rules="[v => !!v || 'Item is required']"
                        label="Constraints on Use"
                        required
                        @change="restrictedSelected"
                ></v-select>
                <v-select
                        v-model="keywords_dictionary"
                        chips
                        :items="keywords_dict_list"
                        :rules="[v => !!v || 'Item is required']"
                        label="Keywords Dictionary"
                        required
                ></v-select>
                <v-combobox
                        v-model="keywords"
                        :items="keywords_list"
                        label="Keywords"
                        multiple
                        chips
                ></v-combobox>
                <v-select
                        v-model="select_constraints"
                        chips
                        :items="constraints_use"
                        :rules="[v => !!v || 'Item is required']"
                        label="Constraints on Use"
                        required
                        @change="restrictedSelected"
                ></v-select>
                <v-select
                        v-show="restricted_visible"
                        v-model="restricted_details_choice"
                        chips
                        :items="restricted_details"
                        label="Restricted Details"
                        required
                ></v-select>

                <v-combobox
                        v-model="coc"
                        :items="coc_values"
                        label="Chain of Custody"
                        multiple
                        chips
                ></v-combobox>
                <!--  v-for="(item, i) in items"
      :key="i" -->
                <!-- <div v-for="(course,i) in coc" :key="i">
                 <v-text-field
                         v-model="coc"
                         :items="coc_values"
                         :key="course"
                         label="Chain of Custody"
                         required
                         clearable
                         hint="Insert Well Known name of Spatial Area or Coordinates Extent from Map Below"
                 ></v-text-field>
                 </div>
                 <v-btn
                         small
                         color="red"
                         dark
                         class="ml-3 mb-3"
                         @click="bumpTheIdx"
                 >
                     Add to Chain
                 </v-btn>-->
                <!--                    <v-textarea
                                            background-color="grey lighten-2"
                                            v-model="chain_of_custody"
                                            filled
                                            label="Chain of Custody"
                                            auto-grow
                                            clearable
                                            hint="Insert the Chain of Custody for the Collection here."
                                    ></v-textarea>-->
                <v-textarea
                        background-color="grey lighten-2"
                        v-model="process_steps"
                        filled
                        label="Process Steps"
                        auto-grow
                        clearable
                        hint="Insert the Process Steps for the Collection here."
                ></v-textarea>
                <v-text-field
                        v-model="distribution_contact"
                        label="Distribution Information"
                        required
                        :rules="[v => !!v || 'Distribution Information is required']"
                        clearable
                        hint="Insert distribution method(s) and any contact information"
                ></v-text-field>
                <!--        <v-select
                                v-model="keywords"
                                multiple
                                chips
                                :items="format_types"
                                :rules="[v => !!v || 'Item is required']"
                                label="Format Types"
                                required
                        ></v-select>

                        <v-checkbox
                                v-model="checkbox"
                                :rules="[v => !!v || 'You must agree to continue!']"
                                label="Do you agree?"
                                required
                        ></v-checkbox>-->
                <v-btn
                        :disabled="!valid"
                        color="success"
                        class="mr-4"
                        @click="validate"
                >
                    Validate
                </v-btn>

                <v-btn
                        color="error"
                        class="mr-4"
                        @click="reset"
                >
                    Reset Form
                </v-btn>

                <v-btn
                        color="warning"
                        @click="resetValidation"
                >
                    Reset Validation
                </v-btn>
            </v-form>
        </v-card>
        <v-card
                class="mx-auto ml-5 mb-2 mt-2"
                max-width="450"
                min-width="450"
                max-height="inherit"
                elevation="3">
            <v-card-title>YAML Output</v-card-title>
            <v-card-text style="background-color: black;color:yellow;text-align: left;" class="pa-2">
                global_attributes: <br/>
                collection_title: <span style="color: antiquewhite">'{{title}}'</span><br/>
                description: <span style="color: antiquewhite">'{{description}}'</span><br/>
                purpose: <span style="color: antiquewhite">'{{purpose}}'</span> <br/>
                format_types: <span style="color: antiquewhite">{{formats_selected}}</span> <br/>
                table_of_contents: <span style="color: antiquewhite">'{{table_of_contents}}'</span><br/>
                spatial_extent: <span style="color: antiquewhite">{{spatial_extent}}</span> <br/>
                publication_date: <span style="color: antiquewhite">{{date}}</span> <br/>
                keywords_dictionary: <span style="color: antiquewhite">'{{keywords_dictionary}}'</span><br/>
                keywords: <span style="color: antiquewhite">{{keywords}}</span> <br/>
                constraints_on_use: <span style="color: antiquewhite" v-text="formatted_use_constraints()"></span> <br/>
                chain_of_custody: <span style="color: antiquewhite">'{{coc}}'</span><br/>
                process_steps: <span style="color: antiquewhite">'{{process_steps}}'</span><br/>
                distribution_information: <span style="color: antiquewhite">'{{distribution_contact}}'</span><br/>
            </v-card-text>
            <v-btn
                    class="mt-3"
                    color="warning"
                    @click="formatted_for_download('metadata1.yaml')"
            >
                Download Metadata
            </v-btn>
        </v-card>
    </v-row>
</template>

<script>
    import "leaflet/dist/leaflet.css";
    import L from "leaflet";
    import * as esri from "esri-leaflet";
    const mapboxKey = "pk.eyJ1IjoiaTE5MjAyMzgiLCJhIjoiY2tudGg4OXo5MDJoMzJxbGR5MXE1MGQ2OCJ9.g2IeZO1evkELGV2hREfnWQ";
    let mapDiv;

    export default {
        name: 'MetadataForm',
        created(){
            //this.$eventHub.$on('toggleAquifers', this.showHideAquifers);
            this.$eventHub.$on('invalidateSizeMap', this.invalidSize);
        },
        data: () => ({
            center: [31.3349, -99.99],
            checkbox: false,
            chain_of_custody_string: null,
            coc_idx: 1,
            coc: [],
            coc_values: [],
            constraints_use: [
                'Public',
                'Restricted'
            ],
            date:(new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10)
            ,
            description: null,
            distribution_contact: null,
            email: '',
            emailRules: [
                    v => !!v || 'E-mail is required',
                    v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
                ],
            formats_selected: null,
            formats_selected_as_string: null,
            format_types: [
                    'Documents',
                    'Geospatial',
                    'Images',
                    'Reports',
                    'Spreadsheets',
                    'Other'
                ],
            items: [
                    'Item 1',
                    'Item 2',
                    'Item 3',
                    'Item 4',
                ],
            keywords: [],
            keywords_as_string: null,
            keywords_dictionary: null,
            keywords_dict_list: [
                    'AGROVOC',
                    'GCMD',
                    'USGS Thesarus',
                    'NWS',
                    'ISO 19115'
                ],
            keywords_list: [],
            menu: false,
            name: '',
            nameRules: [
                    v => !!v || 'Name is required',
                    v => (v && v.length <= 10) || 'Name must be less than 10 characters',
                ],
            process_steps: null,
            purpose: null,
            purposeRules: [
                v => !!v || 'Purpose is required',
            ],
            restricted_visible: false,
            restricted_details:[
                'FOUO',
                'Requires DUA',
                'Display on Web',
                'No Display on Web'
            ],
            restricted_details_choice: null,
            restricted_for_export: null,
            select: null,
            select_constraints: null,
            spatial_extent: null,
            title: null,
            titleRules: [
                v => !!v || 'Title is required',
                v => (v && v.length <= 26) || 'Name must be less than 20 characters',
            ],
            table_of_contents: null,
            update_freq_choice: null,
            valid: true,
        }),

        methods: {
            invalidSize: function(){
                mapDiv.invalidateSize();
                //console.log('Invalid SIZE');
            },
            resetMap: function(){
                mapDiv.setView(this.center,5);
            },
            setupLeafletMap: function () {
                mapDiv = L.map("mapContainer").setView(this.center, 5);
                L.tileLayer("https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=pk.eyJ1IjoiaTE5MjAyMzgiLCJhIjoiY2tudGg4OXo5MDJoMzJxbGR5MXE1MGQ2OCJ9.g2IeZO1evkELGV2hREfnWQ", {
                    attribution: ' Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap<a/> contributors, Imagery © <a href = "https://www.mapbox.com/">Mapbox</a>',
                    mapZoom: 18,
                    id: 'mapbox/navigation-night-v1',
                    tileSize: 512,
                    zoomOffset: -1,
                    accessToken: mapboxKey
                });

                esri.basemapLayer('Streets').addTo(mapDiv);

                //this.testDrive();
                /* mapDiv.on('click',function(){
                     console.log(mapDiv._container.parentElement.parentElement.parentElement.parentNode);
                     //this.this.testDrive();
                 })*/

            },
            getMapExtent(){
                console.log(mapDiv.getBounds()._southWest.lat);
                let extentCaptureWKT = [
                    ""+[mapDiv.getBounds()._southWest.lat, mapDiv.getBounds()._southWest.lng]+"",
                    ""+[mapDiv.getBounds()._northEast.lat, mapDiv.getBounds()._southWest.lng]+"",
                    ""+[mapDiv.getBounds()._northEast.lat, mapDiv.getBounds()._northEast.lng]+"",
                    ""+[mapDiv.getBounds()._southWest.lat, mapDiv.getBounds()._northEast.lng]+"",
                    ""+[mapDiv.getBounds()._southWest.lat, mapDiv.getBounds()._southWest.lng]+""];
                console.log(extentCaptureWKT.toString());
                this.spatial_extent = extentCaptureWKT

            },
            validate () {
                this.$refs.form.validate();

            },
            reset () {
                this.$refs.form.reset();
                this.date =  (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10);
            },
            resetValidation () {
                this.$refs.form.resetValidation()
            },
            restrictedSelected () {
                if(this.select_constraints === 'Restricted'){
                    this.restricted_visible = true;
                } else {
                    this.restricted_visible = false;
                    this.restricted_details_choice = null;
                }
            },
            formatted_coc_download (incomingAry) {
                for (let i = 0; i<incomingAry.length; i++){
                    if(i===0){
                        this.chain_of_custody_string = incomingAry[i];
                    } else {
                        this.chain_of_custody_string += ", "+ incomingAry[i];
                    }
                }
                return this.chain_of_custody_string;
                //} else {
                //    return 'no keywords entered'
                //}
            },
            formatted_data_formats_download(incomingAry){
                for (let i = 0; i<incomingAry.length; i++){
                    if(i===0){
                        this.formats_selected_as_string = incomingAry[i];
                    } else {
                        this.formats_selected_as_string += ", "+ incomingAry[i];
                    }
                }
                return this.formats_selected_as_string;
            },
            formatted_keywords_download (incomingAry) {
                for (let i = 0; i<incomingAry.length; i++){
                    if(i===0){
                        this.keywords_as_string = incomingAry[i];
                    } else {
                        this.keywords_as_string += ", "+ incomingAry[i];
                    }
                }
                return this.keywords_as_string;
                //} else {
                //    return 'no keywords entered'
                //}
            },
            formatted_spatial_extent () {
                if(Array.isArray(this.spatial_extent)){
                    console.log("It is");
                    for (let i = 0; i<this.spatial_extent.length; i++){
                        if(i===0){
                            this.keywords_as_string = this.spatial_extent[i];
                        } else {
                            this.keywords_as_string += ", "+ this.spatial_extent[i];
                        }
                    }
                    return "'" + this.keywords_as_string +"'";
                } else {
                    return "'" + this.spatial_extent +"'";
                }
            },
            formatted_use_constraints () {
                if(this.select_constraints === 'Restricted'){
                    this.restricted_for_export = "'"+this.select_constraints + " > " +this.restricted_details_choice + "'";
                    return "'"+this.select_constraints + " > " +this.restricted_details_choice + "'";
                } else {
                    this.restricted_for_export = this.select_constraints;
                    return this.select_constraints
                }
            },
            formatted_for_download(incomingFileName){
                let someVar = 'global_attributes:' + '\r\n' +
                    '  collection_title: ' + this.title + '\r\n' +
                    '  description: ' + this.description + '\r\n' +
                    '  purpose: ' + this.purpose + '\r\n' +
                    '  format_types: ' + this.formatted_data_formats_download(this.formats_selected) + '\r\n' +
                    '  table_of_contents: ' + '\r\n' +
                    '  spatial_extent: ' + this.formatted_spatial_extent() + '\r\n' +
                    '  publication_date: ' + this.date + '\r\n' +
                    '  keywords_dictionary: ' + this.keywords_dictionary + '\r\n' +
                    '  keywords: ' + this.formatted_keywords_download(this.keywords)  + '\r\n' +
                    '  constraints_on_use: ' + this.restricted_for_export  + '\r\n' +
                    '  chain_of_custody: ' + this.formatted_coc_download(this.coc) +  '\r\n' +
                    '  process_steps: ' + this.process_steps +  '\r\n' +
                    '  distribution_information: ' + this.distribution_contact  +  '\r\n';
                let element = document.createElement('a');
                element.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(someVar));
                element.setAttribute('download', incomingFileName);

                //element.style.display = 'none';
                document.body.appendChild(element);

                element.click();

                document.body.removeChild(element);
                //return someVar
            }
        },
        mounted() {
            this.setupLeafletMap();
            //this.invalidSize();
        },

    }
    /**
     * ((30,-97),(30,-96},(29,-96),(29,-97),(30,-97))
     * map for spatial extent
     * Validation Rules for other required fields
     * Download Button
     * Functions for the other metadata elements to format them for yaml
     */
</script>

<style>
    #mapContainer {
        /* width: 60vw;
         height: 55vh;*/
        width:100%;
        height:300px;
    }
</style>