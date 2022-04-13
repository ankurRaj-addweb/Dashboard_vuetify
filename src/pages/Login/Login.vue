<template>
  <v-app>
    <v-container fluid>
      <v-row no-gutters>
        <v-col cols="7" class="main-part d-none d-md-none d-lg-flex">
          <div class="d-flex">
            <v-img src="@/assets/logo.svg" contain></v-img>
            <p>PILOTCITY ADMIN PANNEL</p>
          </div>
        </v-col>
        <v-col cols="12" lg="5" class="login-part d-flex align-center justify-center">
          <v-row no-gutters class="align-start">
            <v-col cols="12" class="login-part d-flex align-center justify-center flex-column">
              <div class="login-wrapper pt-md-4 pt-0">
                <div id="alert" >{{ alert }} </div>
                    <v-form>
                      <v-container>
                        <v-row class="flex-column">
                          <v-col>
                            <p class="login-slogan display-2 text-center font-weight-medium my-10">Good Morning, User</p>
                            <v-btn height="45" block color="white" elevation="0" class="google text-capitalize">
                              <v-img src="@/assets/google.svg" max-width="30" class="mr-4"></v-img>
                              Sign in with Google</v-btn>
                          </v-col>
                          <v-col cols="12" class="d-flex align-center my-8">
                            <v-divider></v-divider>
                            <span class="px-5"> or </span>
                            <v-divider></v-divider>
                          </v-col>
                          <v-form>
                            <v-col>
                              <v-text-field
                                  v-model="useremail"
                                  :rules="emailRules"
                                  value="admin@flatlogic.com"
                                  label="Email Address"
                                  required
                              ></v-text-field>
                              <v-text-field
                                  v-model="userpassword"
                                  :rules="passRules"
                                  type="password"
                                  label="Password"
                                  hint="At least 6 characters"
                                  required
                              ></v-text-field>

                            </v-col>
                            <v-col class="d-flex justify-space-between">
                              <v-btn
                                  
                                  class="text-capitalize"
                                  large
                                  :disabled="userpassword.length === 0 || useremail.length === 0"
                                  color="primary"
                                  @click="login"
                              >
                                Login</v-btn>
                              <v-btn large text class="text-capitalize primary--text">Forget Password</v-btn>
                            </v-col>
                          </v-form>
                        </v-row>
                      </v-container>
                    </v-form>
                  

                 

               
              </div>
            </v-col>
            <v-col cols="12" class="d-flex justify-center">
              
            </v-col>
          </v-row>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
import axios from 'axios'

  export default {
    name: 'Login',
    data() {
      return {
        alert:"",
        useremail: '',
        emailRules: [
          v => !!v || 'E-mail is required',
          v => /.+@.+/.test(v) || 'E-mail must be valid',
        ],
        userpassword:'',
        passRules: [
          v => !!v || 'Password is required',
          v => v.length >= 6 || 'Min 6 characters'
        ]
      }
    },
    methods: {
      login(){
        // window.localStorage.setItem('authenticated', true);
        // this.$router.push('/dashboard');
        try{
      const put = this;
      //const result = axios
        axios.post(
          "https://authentication-service-dev.api.pilotcity.com/auth0/sign-in",
          {
            type: "email_signin",
            email: this.useremail,
            password: this.userpassword,
          }
        )
        .then(function (response) {
          if (response.status == 200) {
            // localStorage.setItem("user-info",JSON.stringify(response.data[0]))
            if (!put.$store.getters.getAuth) {
              put.$store.commit("toggleAuth");
              console.log("store if");
            }
            put.$router.push({ path: "/dashboard" });
          } else {
            console.log("v-else");
          }
        })
        .catch(function (error) {
          
          console.log(error);
        });
        }catch(error){
          //this.error='Invalid Username/Password!'
          // alert(this.error);
          this.alert = "Invalid user";
          console(this.alert)
        }
      }
    },
    created() {
      if (window.localStorage.getItem('authenticated') === 'true') {
        this.$router.push('/dashboard');
      }
    }
  }

</script>

<style src="./Login.scss" lang="scss"/>
