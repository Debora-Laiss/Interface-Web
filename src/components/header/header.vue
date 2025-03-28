<template>
    <div id="header">
      <router-link id="logo-url" to="/">
        <img id="logo" :src="logo" :alt="alt">
      </router-link>
      <router-link to="/">Home</router-link>
    </div>
  </template>
  
  <script>
  export default {
    name: "Header",
    props: ["logo", "alt"]
  }
  </script>
  <style scoped>
  #header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 1rem;
    background-color: #bad6f1;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    border-radius: 0.3rem;
  }
  
  #logo-url {
    text-decoration: none;
  }
  
  #logo {
    height: 50px;
    max-width: 150px;
    object-fit: contain;
  }
  
  #logo-url + a {
    color: #333;
    text-decoration: none;
    font-weight: 600;
  }
  </style>