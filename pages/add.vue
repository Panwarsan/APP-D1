<template>
  <v-parallax src="https://webstatic.hoyoverse.com/upload/op-public/2023/08/04/d084526694ea2ab4d6de1d642c2322c4_3937311610197244750.png">
  <div class="text-center pa-4">
   <v-dialog
     v-model="dialog_error"
     width="auto"
   >
     
     <v-card
       max-width="400"
       v-model="status_save"
       prepend-icon="mdi-update"
       text= "ไม่สามารถบันทึกได้"
       title="สถานะการบันทึก"
     >
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
 <div class="text-center pa-4">
   <v-dialog
     v-model="dialog"
     width="auto"
   >
     
     <v-card
       max-width="400"
       v-model="status_save"
       prepend-icon="mdi-update"
       text= "บันทึกสำเร็จ"
       title="สถานะการบันทึก"
     >
       <template v-slot:actions>
         <v-btn
           class="ms-auto"
           text="Ok"
           @click="dialog = false"
         ></v-btn>
       </template>
     </v-card>
   </v-dialog>
  </div>
   <v-sheet class="mx-auto" width="300">
 
     <v-form ref="form">
       <v-text-field
         v-model="username"
         label="UserName"
         required
       ></v-text-field>
       <v-text-field
         v-model="password"
         label="Password"
         required
       ></v-text-field><v-text-field
         v-model="email"
         label="Email"
         required
       ></v-text-field>
       <v-text-field
         v-model="picture"
         label="Picture"
         required
       ></v-text-field>
     
       <v-select
         v-model="status"   
         :items="items"
         :rules="[v => !!v || 'Item is required']"
         label="Item"
         required
       ></v-select>      
       <div class="d-flex flex-column">
         <v-btn
           class="mt-4"
           color="success"
           block
           @click="save"
         >
           SAVE
         </v-btn>
 
         <v-btn
           class="mt-4"
           color="error"
           block
           @click="reset"
         >
           Reset Form
         </v-btn>
 
       </div>
     </v-form>
   </v-sheet>
  </v-parallax>
 </template>
 <script>
 import axios from 'axios'
 export default {
   data: () => ({
     dialog_error: false,
     status_save:'',
     username: '',
     password: '',
     email: '',
     status: '',
     picture: '',
     select: null,
     items: [
       'Yes',
       'No',      
     ],
     dialog: false,
     stds: '',
   }),
   methods: {
      async save () {
       let students = {
        username: this.username,
        password: this.password,
        email: this.email,
        status: this.status,
        picture: this.picture,
       }
       console.log(students)
       const response = await axios.post('http://localhost:7000/insert',students)
       this.stds = response.data
       console.log("stds=",this.stds.status)
       if(this.stds.status == 'ok'){
        this.dialog = true 
        console.log('บันทึกสำเร็จ')    
        this.status_save = 'บันทึกสำเร็จครับ'       
       }
       if(this.stds.status == '0'){
        this.dialog_error = true 
        console.log('บันทึกไม่สำเร็จ')    
        this.status_save = 'บันทึกไม่สำเร็จครับ'       
       }
    },
     reset () {
       this.$refs.form.reset()
     },
   },
 }
</script>