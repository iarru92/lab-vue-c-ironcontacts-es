<template>
  <div id="app">
    <h1>Contact List</h1>
    <div class="buttons">
      <button @click="addRandomContact">Add Random Contact</button>
      <button @click="sortByName">Sort by Name</button>
      <button @click="sortByPopularity">Sort by Popularity</button>
    </div>
    <table>
      <thead>
        <tr>
          <th>Picture</th>
          <th>Name</th>
          <th>Popularity</th>
          <th>Won an Oscar</th>
          <th>Won an Emmy</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="contact in contacts" :key="contact.id">
          <td><img :src="contact.pictureUrl" alt="contact picture" class="contact-picture" /></td>
          <td>{{ contact.name }}</td>
          <td>{{ contact.popularity }}</td>
          <td v-if="contact.wonOscar">🏆</td>
          <td v-else></td>
          <td v-if="contact.wonEmmy">🏆</td>
          <td v-else></td>
          <td><button @click="deleteContact(contact.id)" class="delete-button">Delete</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { ref } from 'vue';
import contactsData from './contacts.json';

export default {
  name: 'App',
  setup() {
    const contacts = ref(contactsData.slice(0, 5));
    const remainingContacts = ref(contactsData.slice(5));

    const addRandomContact = () => {
      if (remainingContacts.value.length === 0) {
        return;
      }

      const randomIndex = Math.floor(Math.random() * remainingContacts.value.length);
      const randomContact = remainingContacts.value.splice(randomIndex, 1)[0];
      contacts.value.push(randomContact);
    };

    const sortByName = () => {
      contacts.value.sort((a, b) => a.name.localeCompare(b.name));
    };

    const sortByPopularity = () => {
      contacts.value.sort((a, b) => b.popularity - a.popularity);
    };

    const deleteContact = (id) => {
      contacts.value = contacts.value.filter(contact => contact.id !== id);
    };

    return {
      contacts,
      addRandomContact,
      sortByName,
      sortByPopularity,
      deleteContact
    };
  }
};
</script>

<style>
#app {
  font-family: 'Arial', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 40px;
  color: #444;
}

h1 {
  font-size: 2.5em;
  margin-bottom: 20px;
  color: #2c3e50;
}

.buttons {
  margin-bottom: 20px;
}

button {
  margin: 0 10px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  border: none;
  border-radius: 5px;
  background-color: #3498db;
  color: white;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #2980b9;
}

table {
  margin: 0 auto;
  border-collapse: collapse;
  width: 90%;
  max-width: 1000px;
  background: #ecf0f1;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 2px 15px rgba(0, 0, 0, 0.1);
}

th, td {
  border: 1px solid #ddd;
  text-align: left;
  padding: 12px 15px;
}

th {
  background-color: #34495e;
  color: white;
}

tr {
  transition: background-color 0.3s;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}

tr:hover {
  background-color: #e1e1e1;
}

.contact-picture {
  width: 100px;
  border-radius: 5px;
}

.delete-button {
  background-color: #e74c3c;
  transition: background-color 0.3s;
}

.delete-button:hover {
  background-color: #c0392b;
}
</style>
