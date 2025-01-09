<script setup>
import { ref } from 'vue'
import axios from 'axios'

const formData = ref({
  name: '',
  email: '',
  message: ''
})

const handleSubmit = async () => {
  if (navigator.onLine) {
    try {
      const response = await axios.post('http://pwa.test/api/submit', formData.value)
      console.log('Form submitted successfully:', response.data)
    } catch (error) {
      console.error('Error submitting form:', error)
    }
  } else {
    localStorage.setItem('formData', JSON.stringify(formData.value))
    alert('You are offline. Form data has been saved locally.')
  }
}

const resendData = async () => {
  const savedData = localStorage.getItem('formData')
  if (savedData) {
    try {
      const response = await axios.post('http://pwa.test/api/submit', JSON.parse(savedData))
      console.log('Form submitted successfully:', response.data)
      localStorage.removeItem('formData')
    } catch (error) {
      console.error('Error resending form data:', error)
    }
  }
}

window.addEventListener('online', resendData)
</script>

<template>
  <div class="home-container">
    <h1>Home</h1>
    <p>Welcome to the Home page.</p>

    <form @submit.prevent="handleSubmit">
      <div>
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="formData.name" required />
      </div>
      <div>
        <label for="email">Email:</label>
        <input type="email" id="email" v-model="formData.email" required />
      </div>
      <div>
        <label for="message">Message:</label>
        <textarea id="message" v-model="formData.message" required></textarea>
      </div>
      <button type="submit">Submit</button>
    </form>
  </div>
</template>

<style scoped>
.home-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  width: 50vw;
  background-color: #070707;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

form {
  display: flex;
  flex-direction: column;
  gap: 10px;
  width: 100%;
  max-width: 600px;
}

label {
  font-weight: bold;
}

input, textarea {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
</style>