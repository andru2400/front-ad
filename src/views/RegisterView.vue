<template>
  <div>          

    <div class="alert alert-primary" role="alert">
        <h3 v-text="'consumiendo de = '+urlConnection"></h3>  
        <div v-if="proyect !== ''">
            <h3 v-text="'Proyecto = '+proyect"></h3>
        </div>    
    </div>

    <!-- --------------------------------------------------------------------------------------- -->
    <section class="w-100 p-4 d-flex justify-content-center pb-4">
      <form style="width: 22rem;" @submit.prevent="register">

        <!-- text input -->
        <div class="form-outline mb-4">
          <input type="text" id="name" v-model="form.name" name="name" class="form-control">
          <label class="form-label" for="form2Example1" style="margin-left: 0px;">Name</label>
        <div class="form-notch"><div class="form-notch-leading" style="width: 9px;"></div><div class="form-notch-middle" style="width: 88.8px;"></div><div class="form-notch-trailing"></div></div></div>

        <!-- Email input -->
        <div class="form-outline mb-4">
          <input type="email" id="email" v-model="form.email" name="email" class="form-control">
          <label class="form-label" for="form2Example1" style="margin-left: 0px;">Email address</label>
        <div class="form-notch"><div class="form-notch-leading" style="width: 9px;"></div><div class="form-notch-middle" style="width: 88.8px;"></div><div class="form-notch-trailing"></div></div></div>

        <!-- Password input -->
        <div class="form-outline mb-4">
          <input type="password" v-model="form.password"  name="email" id="name" class="form-control">
          <label class="form-label" for="form2Example2" style="margin-left: 0px;">Password</label>
        <div class="form-notch"><div class="form-notch-leading" style="width: 9px;"></div><div class="form-notch-middle" style="width: 64px;"></div><div class="form-notch-trailing"></div></div></div>
        
        <!-- Password input -->
        <div class="form-outline mb-4">
          <input type="password_confirmation" v-model="form.password_confirmation"  name="email" id="name" class="form-control">
          <label class="form-label" for="form2Example2" style="margin-left: 0px;">Password confirmation</label>
        <div class="form-notch"><div class="form-notch-leading" style="width: 9px;"></div><div class="form-notch-middle" style="width: 64px;"></div><div class="form-notch-trailing"></div></div></div>
        
        <!-- Submit button -->
        <button class="btn btn-primary btn-block mb-4">Register</button>

      </form>
    </section>
    <div class="alert alert-danger" role="alert" v-if="error !== ''">
        {{error}}
    </div>
    <!-- --------------------------------------------------------------------------------------- -->


  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
export default {
    mounted: function(){
        this.getNumF()
    },
    data: function(){
        return { 
            form: { 
                name: '',
                email: '',
                password: '',
                password_confirmation: '',
            }, 
            urlConnection : process.env.VUE_APP_CONNECTION,            
            proyect: '',
            response: '',
            error: ''
        }
    },
    methods: { 
        register() { 
            console.log("Enviando registro ...", this.form);
            let e = this;          
            axios.post(this.urlConnection+'api/register',{name: this.form.name, 
                                                          email:this.form.email, 
                                                          password:this.form.password, 
                                                          password_confirmation:this.form.password_confirmation})
                    .then(function (response){                        
                        if(response.data.status == 1){                                                                                
                            e.token = response.data                        
                        } else if(response.data.status == 0) {
                            e.error = response.data
                        }
                    })
                    .catch(function (error) {
                        // handle error
                        e.error = error.response.data;                        
                        console.log(error);
                    })
        },
        getNumF() {
            let e = this;            
            axios.get(this.urlConnection+'api/random')
                    .then(function (response){                        
                        if(response.data.status == 'ok'){                                                                                
                            e.proyect = response.data.proyecto;                         
                        }
                    })
                    .catch(function (error) {
                        // handle error
                        console.log(error);
                    })
        }
    }
}
</script>

<style scoped>    
</style>