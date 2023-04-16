<template>
  <div>
    <h2>Register</h2>
    <form>
      <label for="newUsername">Username:</label>
      <input id="newUsername" v-model="newUsername" type="text">
      <br>
      <label for="newPassword">Password:</label>
      <input id="newPassword" v-model="newPassword" type="password">
      <br>
      <label for="confirmPassword">Confirm Password:</label>
      <input id="confirmPassword" v-model="confirmPassword" type="password">
      <br>
      <button type="button" @click="submitRegistration">
        Register
      </button>
      <button type="button" @click="$emit('close')">
        Cancel
      </button>
    </form>
  </div>
</template>

<script lang="ts">
export default {

  data () {
    return {
      newUsername: '',
      newPassword: '',
      confirmPassword: ''
    }
  },
  methods: {
    submitRegistration () {
      // Handle registration submission here
      // Make sure to validate the input fields before submitting
    }
  }
}
</script>
