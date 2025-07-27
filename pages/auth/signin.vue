<template>
  <div class=" d-flex mx-auto justify-center align-center" style="height:100vh;">
    <v-card width="500">
      <h1 style="text-align: center;">LOG IN</h1>
      <v-card-text>
        <v-form>

          <v-text-field label="Email" prepend-inner-icon="mdi-email" outlined rounded></v-text-field>

          <v-text-field label="Password" type="Password" prepend-inner-icon="mdi-lock" outlined rounded></v-text-field>
          <v-btn color="blue" size="large" variant="elevated" block rounded>
            Sign in
          </v-btn>

          <div class="my-5 d-flex align-center">
            <v-divider class="flex-grow-1"></v-divider>
            <span class="mx-3">OR</span>
            <v-divider class="flex-grow-1"></v-divider>
          </div>

          <v-btn color="red" size="large" variant="elevated" @click="signInwithGoogle()" block rounded>           
            SIGN IN WITH GOOGLE
          </v-btn>

          <br>
          
          <v-btn color="blue" size="large" variant="elevated" @click="signInWithDiscord()" block rounded>        
            SIGN IN WITH DISCORD
          </v-btn>
        </v-form>

      </v-card-text>
    </v-card>

  </div>
</template>
<script>
export default {
  layout: "auth",
  middleware: 'guest',
  methods: {
    signInwithGoogle() {
      this.$auth.loginWith('google')
      console.log("Signin with google")
    },
    signInWithDiscord() {
      this.$auth.loginWith('discord')
    }
  }
}

</script>
