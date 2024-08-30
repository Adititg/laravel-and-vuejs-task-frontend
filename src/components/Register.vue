<template>
    <div>
        <h2>Register</h2>
        <p class="text-danger" v-if="error">{{ error }}</p>
        <p class="text-success"  v-if="success">{{ success }}</p>
        <div class="form-div" >
            
            <form @submit.prevent="register">
                
                <input class="width p-1" v-model="name" type="text" placeholder="Name" required /><br>
                
                <input class="width p-1" v-model="email" type="email" placeholder="Email"  />
                
                <input class="width p-1" v-model="password" type="password" placeholder="Password" required />
                
                <input class="width p-1" v-model="password_confirmation" type="password" placeholder="Confirm Password" required /><br>
                <button type="submit" class="form-btn">Register</button>
                <button @click="goToLogin">Login</button>
                
            </form>
        </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        name: '',
        email: '',
        password: '',
        password_confirmation: '',
        error: '',
        success:'',
      };
    },
    methods: {
        async register() {
            this.error = '';
            this.success = '';
            try {
            const response = await axios.post('http://localhost:8000/api/register', {
                name: this.name,
                email: this.email,
                password: this.password,
                password_confirmation: this.password_confirmation
            });
            this.error = '';
            console.log('Registration successful:', response.data);
            this.success = response.data.message;
            } catch (error) {
                
                if(error.response.data.email) {
                    this.error = error.response.data.email[0];
                } else if(error.response.data.password) {
                    this.error = error.response.data.password[0];
                } else {
                    this.error = 'Error occured.'
                }
                
            }
        },
        goToLogin() {
            this.$router.push('/login');
        }
    }
  };
  </script>