<template>
  <div>    
      <h3 v-text="'url Back = '+urlConnection"></h3>  
      <div v-if="proyect !== ''">
        <h3 v-text="'Proyecto = '+proyect"></h3>
      </div>    

    <form @submit.prevent="login">
        <input v-model="form.email" type="email" placeholder="Email ...">
        <br/>
        <input v-model="form.password" type="password" placeholder="Password ...">
        <br/>
        <button>Login</button>
    </form>
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
            proyect: ''
        }
    },
    methods: { 
        login() { 
            console.log("Enviando login ...", this.form)
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