<template>
 <div id="forgot">
<Nav/>
   <form @submit.prevent="handleSubmit"> 
        <div class="container mt-5">
            <div id="box" class="form pt-4 border-0 rounded-4 shadow-lg overflow-hidden">
                <div class="text-center">
                <img src="../../assets/mycollection/png/user/user-7.png" class="mb-2">
                </div>
      <h3>Resetuj hasło</h3>
<div class="form-group">
    <label class="m-2" style="font-size:14px;">Podaj e-mail powiązany z kontem:</label>
    <input v-model="email" type="email" class="form-control" placeholder="Email" required>
</div>
     <div class="merge d-flex justify-content-center flex-column">
      <div class="text-center">
      <button style="width:120px;" class="mt-4 btn btn-light shadow-sm btn-block btn-lg mb-4">Resetuj</button>
    
           </div>
            </div>       
             </div>
             </div>
        </form>
  </div>
  
</template>

<script>
import axios from 'axios'
import Nav from './Nav.vue'
export default {

    components: { Nav },
    name: 'Forgot',
    data(){

        return { 
            email: ''
        }
    },
    methods: {
        
        async handleSubmit(){
 
            await axios.post('forgot',{
            email: this.email
            
        }).catch( function(error){
            
            if(error.response){
                
                alert(error.response.data.message);

            }
            }
        );
         alert('Sprawdź swoją skrzynkę e-mail!');
        }
        
    }
     
}
</script>

<style>

</style>