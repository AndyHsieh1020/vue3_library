<template>
    <div>
      <h2>Register</h2>
      <form @submit.prevent="register">
        <div>
          <label for="username">Username:</label>
          <input type="text" id="username" v-model="username">
        </div>
        <div>
          <label for="password">Password:</label>
          <input type="password" id="password" v-model="password">
        </div>
        <button type="submit">Register</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        username: '',
        password: ''
      }
    },
    methods: {
      register() {
        
        fetch('http://localhost:8080/api/register', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ username: this.username, password: this.password })
        })
        .then(response => {
          if (response.ok) {
            return response.json();
          } else {
            throw new Error('Registration failed');
          }
        })
        .then(data => {
          this.$router.push('/login');
        })
        .catch(error => {
          console.error('Registration error:', error);
          alert('Registration failed');
        });
      }
    }
  }
  </script>
  