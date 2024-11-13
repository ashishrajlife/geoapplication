<template>
    <div class="login-page">
      <div class="login-card">
        <h2>Login</h2>
        <form>
          <div class="input-group">
            <label for="email">Email</label>
            <input type="text" id="email" v-model="email"  />
          </div>
          <div class="input-group">
            <label for="password">Password</label>
            <input type="password" id="password" v-model="password" />
          </div>
          <button @click="login">Login</button>
        </form>
        <p class="login-link"> No Account ? <router-link to="/">Signup</router-link></p>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import { toast } from 'vue3-toastify';
  import 'vue3-toastify/dist/index.css';

  export default {
    name: 'LoginView',
    data() {
      return {
        email: '',
        password: ''
      };
    },
    methods: {
     async login() {
      if(this.email == ''){
        toast.error('username cannot be empty',{ autoClose:3500});
        return;
      }
      if(this.password == ''){
        toast.error('password cannot be empty',{ autoClose:3500});
        return;
      }
       console.log(this.email,this.password)
        let result = await axios.get(`http://localhost:3000/user?email=${this.email}&password=${this.password}`)
        console.log(result)
        if (result.status == 200 && result.data.length>0) {
          toast.success('Logging in',{ autoClose:1000});
          // this.$router.push({ name: 'Home' });
          setTimeout(() => {
            this.$router.push({ name: 'Home' });
  }, 1500);
        } else {
          toast.error('Username/Password is Wrong !',{ autoClose:2000});
        }
    }
}
  };
  </script>
  
  <style scoped>
  .login-page {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
    background-image: url('../../src/assets/images/space.png');
    font-family: Arial, sans-serif;
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
  }

  .login-card {
    background: rgb(48, 46, 46);
    padding: 2rem;
    border: 1.5px solid yellow;
    width: 100%;
    max-width: 400px;
    border-radius: 8px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    text-align: center;
  }
  .login-card h2 {
    color: white;
    margin-bottom: 1.5rem;
    font-weight: bold;
    font-family: Arial, Helvetica, sans-serif;
  }

  .input-group {
    margin-bottom: 1rem;
    text-align: left;
  }
  
  .input-group label {
    display: block;
    font-weight: 500;
    color: white;
    margin-bottom: 0.3rem;
  }
  
  .input-group input {
    width: 100%;
    padding: 0.2rem;
    /* border-radius: 5px; */
    border: 1px solid #ddd;
    font-size: 1rem;
  }

  button {
    width: 100%;
    margin-top: 10px;
    padding: 0.8rem;
    background-color: #6c5ce7;
    color: white;
    font-size: 1rem;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  button:hover {
    background-color: #4b3cb1;
  }
  .login-link {
  margin-top: 20px;
  font-size: 0.9rem;
  color: white;
}

.login-link a {
  color: #4b9cd3;
  text-decoration: none;
}

.login-link a:hover {
  text-decoration: underline;
}
  </style>
  