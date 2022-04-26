<template>
  <div>    
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
export default {
    data: () => ({
        form: { 
            email: '',
            password: ''
        }
    }),
    methods: { 
        login() { 
            console.log("Enviando login ...", this.form)
        }
    }
};
</script>

<style scoped>
    input {
        margin: 2px;
    }
</style>