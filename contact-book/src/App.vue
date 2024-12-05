<script setup>
import ContactCard from './components/contact.vue'
import { array } from './components/Contacts.vue'
let contacts = array
let contactinfo = {}

if (localStorage.getItem('contacts')) {
  contacts = JSON.parse(localStorage.getItem('contacts'))
}

contacts.sort(compareLast)
function compareLast(a, b) {
  if (a.name.last < b.name.last){
    return -1
  }
  if (a.name.last > b.name.last){
    return 1
  }
  return 0
}
function compareFirst(a, b) {
  if (a.name.first < b.name.first){
    return -1
  }
  if (a.name.first > b.name.first){
    return 1
  }
  return 0
}
function first() {
  contacts.sort(compareFirst)
}
function last() {
  contacts.sort(compareLast)
}
function addContact() {
  let form = document.querySelector("#addform")
  let formData = new FormData(form)
  contacts.push({
    "name":{"first":formData.get('firstname').toLowerCase(),"last":formData.get('lastname').toLowerCase()},
    "email":formData.get('email').toLowerCase()
  })
  localStorage.setItem('contacts', JSON.stringify(contacts))
  console.log(contacts)
  render=1
}
function editContact(a) {
  let form = document.querySelector("#editform")
  let formData = new FormData(form)
  let index = contacts.indexOf(a)
  contacts.splice(index, 1, {
    "name":{"first":formData.get('firstname').toLowerCase(),"last":formData.get('lastname').toLowerCase()},
    "email":formData.get('email').toLowerCase()
  })
  localStorage.setItem('contacts', JSON.stringify(contacts))
}
function deleteContact(a) {
  let index = contacts.indexOf(a)
  contacts.splice(index, 1)
  localStorage.setItem('contacts', JSON.stringify(contacts))
}
function details(a) {
  contactinfo = a
}
</script>

<template>
  <div v-if="render==1">
    <button id="fbtn" @click="first">First Name</button><button id="lbtn" @click="last">Last Name</button>
    <div class="cards">
      <button @click="render=3,details(contact)" v-for="contact in contacts" :class="'card'+contacts.indexOf(contact)+' contactcard'">
        <ContactCard :props="contact"></ContactCard>
      </button>
      <button class="addbtn" @click="render=2">New Contact</button>
    </div> 
  </div>
  <div v-if="render==2">
    <form id="addform">
      <label for="firstname">First Name</label>
      <input type="text" name="firstname">
      <label for="lastname">Last Name</label>
      <input type="text" name="lastname">
      <label for="email">Email</label>
      <input type="email" name="email">
      <button @click="addContact">Add Contact</button>
      <button @click="render=1">Back to Contacts</button>
    </form>
  </div>
  <div v-if="render==3">
    <ContactCard :props="contactinfo"></ContactCard>
    <button class="dbtn" @click="render=1">Back to Contacts</button>
    <button class="dbtn" @click="render=4">Edit Contact</button>
    <button class="dbtn" @click="render=1,deleteContact(contactinfo)">Delete Contact</button>
  </div>
  <div v-if="render==4">
    <form id="editform">
      <label for="firstname">First Name</label>
      <input type="text" name="firstname" :placeholder="[contactinfo.name.first]">
      <label for="lastname">Last Name</label>
      <input type="text" name="lastname" :placeholder="[contactinfo.name.last]">
      <label for="email">Email</label>
      <input type="email" name="email" :placeholder="[contactinfo.email]">
      <button @click="render=1,editContact(contactinfo)">Change Contact</button>
      <button @click="render=1">Back to Contacts</button>
    </form>
  </div>
</template>

<style scoped>
.cards {
  width: 85vw;
}
.addbtn {
  background-color: rgb(69, 69, 69);
  width: 100%;
  border-radius: 15px;
  padding: 5px;
  margin: 0;
  min-height: 106px;
}
#addform label, #addform button, #addform input {
  display: block;
  text-align: center;
  margin: 15px auto;
}
#editform label, #editform button, #editform input {
  display: block;
  text-align: center;
  margin: 15px auto;
}
.dbtn {
  display: block;
  text-align: center;
  margin: 15px auto;
}
#fbtn, #lbtn {
  margin: 10px;
}
</style>

<script>
export default {
  data() {
   return {
     contacts: array,
     render: 1
   }
  }
}
</script>