<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
    <form v-on:submit.prevent="updateUser">
             <input type="hidden" id="id" name="id"  v-model="user.id"><br>
  Name:    <input type="text" id="name" name="name"  v-model="user.name"><br>
  Email:   <input type="text" id="email" name="email" v-model="user.email"><br><br>
  Image:   <img :src="user.avatar"><input  id="photo" name="photo" type="file" @change="onfilechange" /><br>
   <button @click="updateUser">Update</button>
</form> 
       </v-col>
  </v-row>
</template>
<style scoped>
input{
  color:white;

  

}
</style>

<script>
export default {
  name: "user",
   data() {
        return {
            data: {
                name: '',
                email:'',
                avatar: '',
                selectedfile:''
            },
            user: {},
        }
       
    },
  methods: {
     onfilechange(event){
      this.selectedfile = event.target.files[0];
      if(this.selectedfile!=""){
          this.avatar=this.selectedfile
          console.log(this.avatar)
      }
  },
  updateUser(e) {
            this.$axios
                .put(`https://62d7f3219c8b5185c77f9c74.mockapi.io/users/${this.$route.params.id}`, { 
                    name: this.user.name,
                    email: this.user.email
                })
                .then((resp) => {
                    //this.$toast.success('User updated Successfully').goAway(1000)
                    this.$router.push(`/users`)
                    // this.user = resp.user
                    // this.user.name = resp.user.name
                    // console.warn(resp.user)
                })
                .catch((error) => {
                    console.log(error)
                })
        }
        // deleteUser(e) {
        //     this.$axios
        //         .get(`https://62d7f3219c8b5185c77f9c74.mockapi.io/users/${this.$route.params.id}`, { 
        //         })
        //         .then((resp) => {
                    
        //             this.$router.push(`/users`)
        //             // this.user = resp.user
        //             // this.user.name = resp.user.name
        //             // console.warn(resp.user)
        //         })
        //         .catch((error) => {
        //             console.log(error)
        //         })
        // },
   
   

  
},
  mounted() {
    this.$axios
      .get(
        `https://62d7f3219c8b5185c77f9c74.mockapi.io/users/${this.$route.params.id}`
      )
      .then((resp) => {
        this.user = resp.data
        this.data.name = resp.data.name
        console.warn(resp.data)
      })
      .catch((error) => {
        console.log(error)
      });      
  },
};
</script>
