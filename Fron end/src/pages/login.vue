<template>
  <div class="login-container">
    <h1 class="login-title">Sign in</h1>
    <div class="form-container">
      <input
        type="text"
        required
        v-model="username"
        class="input-field"
        placeholder="Username"
      />
      <input
        type="password"
        required
        v-model="password"
        class="input-field"
        placeholder="Password"
      />
      <button @click="handleLogin" class="login-button">
        Login
      </button>
    </div>
  </div>
</template>

<style scoped>
.login-container {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ff0000;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.login-title {
  font-size: 24px;
  text-align: center;
  margin-bottom: 20px;
}

.form-container {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.input-field {
  width: 100%;
  padding: 10px;
  border: 1px solid #ff0000;
  border-radius: 4px;
}

.login-button {
  background-color: #dc3434;
  color: #fff;
  padding: 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.login-button:hover {
  background-color: #bd2727;
}
</style>

<script setup lang="ts">
import { ref } from "vue";
import { useRouter } from "vue-router";
import { useAuthStore } from "../stores/auth";
import axios from "axios";
import Swal from 'sweetalert2';

const auth = useAuthStore();
const username = ref("");
const password = ref("");
const router = useRouter();

const handleLogin = async () => {
  try {
    const response = await axios.post("http://localhost:3000/login", {
      username: username.value,
      password: password.value,
    });

    if (response.data.status === "success") {
      Swal.fire({
        title: 'Login Success!',
        text: 'Welcome back!',
        icon: 'success',
        confirmButtonText: 'Cool'
      });
      auth.login(username.value);
      router.push('/');
    } else if (response.data.status === "error, username not found" || response.data.status === "error, wrong password") {
      Swal.fire({
        title: 'Error',
        text: response.data.message || 'Something went wrong!',
        icon: 'warning',
        confirmButtonText: 'Try again'
      });
    }

    console.log(response);
  } catch (error) {
    console.error("Error during login:", error);
  }
};
</script>
