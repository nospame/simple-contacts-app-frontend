<script>
import axios from 'axios'
export default {
  data: function () {
    return {
      message: "Welcome to the Contacts App!",
      contacts: [],
      newContact: {},
      currentContact: {},
      errors: []
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

    },
    createContact: function (contact) {
      axios.post('/contacts', contact)
        .then(response => {
          console.log(response.data);
          this.contacts.push(response.data);
          this.errors = [];
          this.newContact = {};
        })
        .catch(errors => {
          this.errors = errors.response.data.errors;
          console.log(this.errors);
        })
    },
    showContact: function (contact) {
      this.currentContact = contact;
      console.log(this.currentContact);
      document.querySelector("#contact-details").showModal();
    },
    updateContact: function (contact) {
      axios.patch(`/contacts/${contact.id}.json`, contact)
        .then(response => {
          console.log(response.data);
          this.errors = [];
        })
        .catch(errors => {
          this.errors = errors.response.data.errors;
          console.log(this.errors);
        })
    },
    deleteContact: function (contact) {
      console.log(contact.id);
      axios.delete(`/contacts/${contact.id}.json`)
        .then(response => {
          console.log(response.data);
          this.contacts.splice(this.contacts.indexOf(contact), 1);
          this.errors = [];
        })
    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>
      First Name:
      <input v-model="newContact.first_name" />
      <br />Last Name:
      <input v-model="newContact.last_name" />
      <br />Email:
      <input v-model="newContact.email" />
      <br />Phone Number:
      <input v-model="newContact.phone_number" />
      <br />Image URL
      <input v-model="newContact.image" />
    </p>
    <small v-for="error in errors">
      {{ error }}
      <br />
    </small>
    <button v-on:click="createContact(newContact)">Add Contact</button>
    <div v-for="contact in contacts" id="contacts-list" v-bind:key="contact.id">
      <p>
        <img v-bind:src="contact.image" />
        <br />
        {{ contact.first_name }} {{ contact.last_name }}
        <br />
        <button v-on:click="showContact(contact)">Details</button>
      </p>
    </div>
    <dialog id="contact-details">
      <form method="dialog">
        <button class="close-dialog">X</button>
        <br />
        <img v-bind:src="currentContact.image" />
        <h2>{{ currentContact.first_name }} {{ currentContact.last_name }}</h2>
        <p>
          First Name:
          <input v-model="currentContact.first_name" />
          <br />Last Name:
          <input v-model="currentContact.last_name" />
          <br />Email:
          <input v-model="currentContact.email" />
          <br />Phone Number:
          <input v-model="currentContact.phone_number" />
          <br />Image URL:
          <input v-model="currentContact.image" />
          <br />
        </p>
        <p>
          <button v-on:click="updateContact(currentContact)">Update</button>
          <button v-on:click="deleteContact(currentContact)" id="delete">Delete Contact</button>
        </p>
      </form>
    </dialog>
  </div>
</template>

<style>
#contacts-list img {
  width: 100px;
}

#contact-details img {
  width: 300px;
}

#contact-details #delete {
  color: red;
}

.close-dialog {
  background-color: white;
  border: none;
  position: absolute;
  right: 10px;
}
</style>