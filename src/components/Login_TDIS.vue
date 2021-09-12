<template>
    <v-card style="height:250px;min-width:400px;max-width:400px;">
        <v-card-title>Login TDIS  <v-chip
                class="ml-6"
                :color="colorLogStatus"
                :text-color="colorLogText"
        > {{loggedInMessage}}
        </v-chip><!--<span style="margin-left: 2px;"> - Not Logged In</span>--></v-card-title>
        <v-row style="margin-left: 0.2em;margin-right:0.1em;">
            <v-col>
                <v-form
                        ref="form"
                        v-model="valid"
                        lazy-validation
                >
                    <v-text-field
                            v-model="username"
                            :counter="20"
                            label="UserName"
                            required
                    ></v-text-field>

                    <v-text-field
                            v-model="password"
                            :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                            :rules="[rules.required, rules.min]"
                            :type="show1 ? 'text' : 'password'"
                            name="input-10-1"
                            label="Normal with hint text"
                            hint="At least 8 characters"
                            counter
                            @click:append="show1 = !show1"
                    ></v-text-field>
                    <v-btn
                            :disabled="!valid"
                            color="success"
                            class="mr-4"
                            @click="getLoggedIn"
                    >
                        Login
                    </v-btn>
                    <v-btn
                            :disabled="!valid"
                            color="success"
                            class="mr-4"
                            @click="logOut"
                    >
                        Logout
                    </v-btn>

                </v-form>
            </v-col>
        </v-row>
        <v-col></v-col>
    </v-card>
</template>

<script>
    import axios from 'axios';
    export default {
        name: "Login_TDIS",
        data: () => ({
            awtToken:null,
            colorLogStatus:'pink',
            colorLogText: "white",
            email:null,
            name:null,
            emailRules: [
                v => !!v || 'E-mail is required',
                v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
            ],
            loginStatus: false,
            loggedInMessage: "Not Logged In",
            nameRules: [
                v => !!v || 'Name is required',
                v => (v && v.length <= 10) || 'Name must be less than 10 characters',
            ],
            password:'password',
            rules: {
                required: value => !!value || 'Required.',
                min: v => v.length >= 8 || 'Min 8 characters',
                emailMatch: () => (`The email and password you entered don't match`),
            },
            show1: false,
            username:null,
            valid: true,
        }),
        methods: {
            validate () {
                this.$refs.form.validate()
            },
            reset () {
                this.$refs.form.reset()
            },
            resetValidation () {
                this.$refs.form.resetValidation()
            },
            getLoggedIn(){
                axios.defaults.headers.post['Access-Control-Allow-Origin'] = '*';
                let urlLogin = 'https://moves.csr.utexas.edu/tdis_backbone_poc/api/auth/signin';
                //let urlLogin = 'https://uws-sec-ng3.csr.utexas.edu/ida_gauges/api/ahps/covid19/counties/Dallas';
                // eslint-disable-next-line no-unused-vars
                let credentials = {   "username":"modBrent3",  "password": "12345678" };
                let cred2 = {};
                cred2.username = this.username;
                cred2.password = this.password;
                console.log(cred2);
                // eslint-disable-next-line no-undef
                //const headers = {"Access-Control-Allow-Origin":"*", 'Content-Type': 'application/x-www-form-urlencoded'};
                const headers = {"Access-Control-Allow-Origin":"*"};
                axios.post(urlLogin,cred2,{headers})
                    .then(response=>{
                        //return response.data
                        //console.log(response.data);
                        //console.log(this.password);
                        this.assignJWTCred(response.data);
                    })
                   .catch(error=>{
                        console.log(error);
                        this.awtToken = null;
                    });
            },
            assignJWTCred(incomingJSON){
                let info = incomingJSON;
                console.log(info.accessToken);
                this.awtToken = info.accessToken;
                this.tryTheGet();

            },
            tryTheGet(){
                let urlLogin = 'https://moves.csr.utexas.edu/tdis_backbone_poc/api/test/mod2/3-31-2021';
                const headers = {"x-access-token": this.awtToken};
                console.log(headers);
                axios.get(urlLogin,{headers})
                    .then(response=>{
                        console.log(response);
                    })
                    .catch(error=>{
                        console.log(error);
                    });
                //displayMetadataPage
                this.loggedInMessage = "Logged In";
                this.colorLogText = 'black';
                this.colorLogStatus = 'light-blue lighten-4';
                this.$eventHub.$emit('displayMetadataPage');

            },
            logOut(){
                this.awtToken = null;
                this.colorLogStatus = 'pink';
                this.colorLogText = 'white';
                this.loggedInMessage = "Logged Out";
                this.$eventHub.$emit('resetCredentials');
            }
        },
    }
</script>

<style scoped>

</style>