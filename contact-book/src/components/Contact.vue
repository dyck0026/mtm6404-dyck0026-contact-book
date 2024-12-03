<script setup>
import { contacts } from './Contacts.vue'
</script>

<template>
    <p>{{props.name.first}} {{props.name.last}}</p>
    <p>{{props.email}}</p>
</template>

<style>
.contactcard {
  background-color: rgb(55, 55, 55);
  width: 100%;
  border-radius: 15px;
  padding: 5px;
  margin: 20px 0;
}
</style>

<script>
export default {
    name: 'ContactCard',
    props: ['props']
}
</script>

