<template>
    <div>
      <h2>Register</h2>
      <form>
        <label for="newUsername">Username:</label>
        <input type="text" id="newUsername" v-model="newUsername">
        <br>
        <label for="newPassword">Password:</label>
        <input type="password" id="newPassword" v-model="newPassword">
        <br>
        <label for="confirmPassword">Confirm Password:</label>
        <input type="password" id="confirmPassword" v-model="confirmPassword">
        <br>
        <button type="button" @click="submitRegistration">Register</button>
        <button type="button" @click="$emit('close')">Cancel</button>
      </form>
    </div>
  </template>
  
  <script lang="ts">
  export default {

    data() {
      return {
        newUsername: '',
        newPassword: '',
        confirmPassword: ''
      }
    },
    methods: {
      submitRegistration() {
        // Handle registration submission here
        // Make sure to validate the input fields before submitting
      }
    }
  }
  </script>