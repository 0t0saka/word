<template>
    <div class=" d-flex mx-auto justify-center align-center" style="height:100vh;" >
       <v-card width="500" >
        <h1 style="text-align: center;">LOG IN</h1>
            <v-card-text>
                <v-form >
                    
                    <v-text-field
                    label="Email"
                    prepend-inner-icon="mdi-email"
                    outlined
                    rounded
                    ></v-text-field>

                     <v-text-field
                    label="Password"
                    type="Password"
                    prepend-inner-icon="mdi-lock"
                    outlined
                    rounded
                    ></v-text-field>
                    <v-btn  color="blue" size="large"  variant="elevated" block rounded>
                        Sign in
                    </v-btn>
                    <v-divider class="my-5">
                       <v-text-field>OR</v-text-field>
                    </v-divider>
                    
                    <v-btn  color="red" size="large" variant="elevated" @click="signInwithGoogle()" block rounded>
                        SIGN IN WITH GOOGLE
                    </v-btn>
                </v-form>
                
            </v-card-text>
       </v-card>
  
    </div>
</template>
<script>
export default{
    layout:"auth",
   middleware: "guest",
    methods:{
        signInwithGoogle(){
           this.$auth.loginWith('google')
           console.log("Signin with google")
        }
    }


}
</script>
 
