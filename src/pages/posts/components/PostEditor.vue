<template>
    <div>
        <GutenTap v-model="content" mode="html" />
    </div>
  </template>
  
  <script setup>
    import { ref } from 'vue'
    import { GutenTap }  from "gutentap"
    import 'gutentap/style.css'

    const content = ref('<h1>fuck</h1>')
  </script>