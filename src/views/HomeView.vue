<script>
import axios from 'axios'
export default {
  data: function () {
    return {
      message: "Welcome to the Contacts App!",
      contacts: []
    };
  },
  created: function () {
    this.indexContacts();
  },
  methods: {
    indexContacts: function () {
      axios.get('/contacts')
        .then(response => {
          console.log(response.data);
          this.contacts = response.data;
        })

    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="contact in contacts">
      <p>{{ contact.first_name }} {{ contact.last_name }}</p>
    </div>
  </div>
</template>

<style></style>