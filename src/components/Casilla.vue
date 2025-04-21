<template>
    <button>
      {{ figura }}
    </button>
  </template>
  
  <script setup lang="ts">
  defineProps({
    figura: String
  })
  </script>
  
  <style scoped>
  button{
    width: 10rem;
    height: 10rem;
    font-size: 5rem;
    background-color: transparent;
    border-color: var(--color--green);
    margin: .5rem;
    cursor: pointer;
  }
  </style>