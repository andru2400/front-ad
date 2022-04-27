<template>
  <div>          

    <div class="alert alert-primary" role="alert">
        <h3 v-text="'consumiendo de = '+urlConnection"></h3>  
        <div v-if="proyect !== ''">
            <h3 v-text="'Proyecto = '+proyect"></h3>
        </div>    

        <div v-if="token !== ''">
            <h3 v-text="'Token = '+token"></h3>
            <button type="submit" @click="logout">
                Logout
            </button>
        </div>    
    </div>

    <!-- --------------------------------------------------------------------------------------- -->
    <section class="w-100 p-4 d-flex justify-content-center pb-4">
      <form style="width: 22rem;" @submit.prevent="login">
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

        <!-- 2 column grid layout for inline styling -->
        <div class="row mb-4">
          <div class="col d-flex justify-content-center">
            <!-- Checkbox -->
            <div class="form-check">
              <input class="form-check-input" type="checkbox" value="" id="form2Example31" checked="">
              <label class="form-check-label" for="form2Example31"> Remember me </label>
            </div>
          </div>

          <div class="col">
            <!-- Simple link -->
            <a href="#!">Forgot password?</a>
          </div>
        </div>

        <!-- Submit button -->
        <button class="btn btn-primary btn-block mb-4">Login</button>

        <!-- Register buttons -->
        <div class="text-center">
          <p>Not a member? <a href="#!">Register</a></p>
          <p>or sign up with:</p>
          <button type="button" class="btn btn-link btn-floating mx-1">
            <i class="fab fa-facebook-f"></i>
          </button>

          <button type="button" class="btn btn-link btn-floating mx-1">
            <i class="fab fa-google"></i>
          </button>

          <button type="button" class="btn btn-link btn-floating mx-1">
            <i class="fab fa-twitter"></i>
          </button>

          <button type="button" class="btn btn-link btn-floating mx-1">
            <i class="fab fa-github"></i>
          </button>
        </div>
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
                email: '',
                password: ''
            }, 
            urlConnection : process.env.VUE_APP_CONNECTION,            
            proyect: '',
            token: '',
            error: ''
        }
    },
    methods: { 
        login() { 
            console.log("Enviando login ...", this.form);
            let e = this;          
            axios.post(this.urlConnection+'api/login',{email:this.form.email,password:this.form.password})
                    .then(function (response){                        
                        if(response.data.status == 1){                                                                                
                            e.token = response.data.access_token                        
                        } else if(response.data.status == 0) {
                            e.error = response.data
                        }
                        e.error = '';
                    })
                    .catch(function (error) {
                        // handle error
                        e.error = error.response.data;                        
                        console.log(error);
                    })
        },
        logout() {             
            let e = this;     
            const config = {
                headers: { Authorization: `Bearer ${this.token}` }
            };     
            axios.get(this.urlConnection+'api/logout', config)
                    .then(function (response){                        
                        if(response.data.status == 1){                                                                                
                            e.form.email    = '';
                            e.form.password = '';
                            e.token    = '';
                            e.error = '';
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
    input {
        margin: 2px;
    }
</style>