<template>
  <v-parallax src="https://webstatic.hoyoverse.com/upload/op-public/2023/06/28/df04a3e3737a158c572288862c523a03_5679081464175778403.png">
  <div class="text-center pa-4">
   <v-dialog
     v-model="dialog"
     width="auto"
   >
     <v-card
       max-width="400"
       prepend-icon="mdi-update"
       text="Your application will relaunch automatically after the update is complete."
       title="Update in progress">
       <template v-slot:actions>
         <v-btn
           class="ms-auto"
           text="Ok"
           @click="dialog_error = false"
         ></v-btn>
       </template>
     </v-card>
   </v-dialog>
 </div>
 <div>
   <v-img
     class="mx-auto my-6"
     max-width="228"
     src=""
   ></v-img>
   
   <v-card
     class="mx-auto pa-12 pb-8"
     elevation="8"
     max-width="448"
     rounded="lg"
   >
     <div class="text-subtitle-1 text-medium-emphasis">Account</div>

     <v-text-field
       density="compact"
       placeholder="Email address"
       prepend-inner-icon="mdi-email-outline"
       variant="outlined"
       v-model="email"
     ></v-text-field>

     <div class="text-subtitle-1 text-medium-emphasis d-flex align-center justify-space-between">
       Password

       <a
         class="text-caption text-decoration-none text-blue"
         href="#"
         rel="noopener noreferrer"
         target="_blank"
         
       >
         Forgot login password?</a>
     </div>

     <v-text-field
       :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'"
       :type="visible ? 'text' : 'password'"
       density="compact"
       placeholder="Enter your password"
       prepend-inner-icon="mdi-lock-outline"
       variant="outlined"
       @click:append-inner="visible = !visible"
       v-model="passwd"
     ></v-text-field>

     <v-card
       class="mb-12"
       color="surface-variant"
       variant="tonal"
     >
       <v-card-text class="text-medium-emphasis text-caption">
         Warning: After 3 consecutive failed login attempts, you account will be temporarily locked for three hours. If you must login now, you can also click "Forgot login password?" below to reset the login password.
       </v-card-text>
     </v-card>

     <v-btn
       class="mb-8"
       color="blue"
       size="large"
       variant="tonal"
       block
       @click = "dologin"
     >
       Log In
     </v-btn>

     <v-card-text class="text-center">
       <a
         class="text-blue text-decoration-none"
         href="#"
         rel="noopener noreferrer"
         target="_blank"
       >
         Sign up now <v-icon icon="mdi-chevron-right"></v-icon>
       </a>
     </v-card-text>
   </v-card>
 </div>
</v-parallax>
</template>

<script>
definePageMeta({
  layout: "custom",
});
import axios from 'axios'
export default {
data: () => ({
 visible: false,
 email: '',
 passwd: '',
}
),


mounted() {
       let user = window.sessionStorage.getItem('email')
       console.log('user=' , user) 
     },
methods:{
 
 async dologin(){
     let forms={
         email: this.email,
         passwd: this.passwd
     }
     console.log(this.email)
     console.log(this.passwd)
     const response = await axios.post('http://localhost:7000/login',forms)
     // console.log(response.data.successlogin.statuslogin)
     console.log(response.data)
     if (response.data.statuslogin == 'ok') {
         console.log('showdata')
         window.sessionStorage.setItem("email", response.data.successlogin.email)
         this.$router.replace('/home')
     }else{
       console.log('login_error')
       this.dialog_error = true;
       setTimeout(() => {
       this.dialog_error = false; 
       }, 2000);
       this.$router.replace("/login");
     
     }
 },
}
}
</script>