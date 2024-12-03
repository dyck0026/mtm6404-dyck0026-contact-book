<script setup>
import ContactCard from './components/Contact.vue'
import { contacts } from './components/Contacts.vue'
</script>

<template>
  <div class="cards">
    <div v-for="contact in contacts" :class="'card'+contacts.indexOf(contact)+' contactcard'">
    <ContactCard :props="contact"></ContactCard>
  </div>
  </div>
  
</template>

<style scoped>
.cards {
  width: 85vw;
}
</style>
