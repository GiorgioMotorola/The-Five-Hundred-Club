<template>
    <div>
      <img :src="image" alt="Album Cover">
    </div>
  </template>
  
  <script>
  export default {
    props: {
      image: {
        type: String,
        required: true
      }
    }
  };
  </script>
  
  <style scoped>
  </style>
  