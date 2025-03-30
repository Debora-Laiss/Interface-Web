<template>
  <div id="header">
    <router-link id="logo-url" to="/">
      <img id="logo" :src="logo" :alt="alt">
    </router-link>
    <router-link to="/">Home</router-link>
  </div>
</template>

<script>
import logoImg from "../../assets/logo.png"; 
export default {
  name: "Header",
  data() {
    return {
      logo: logoImg,
      alt: "Logo da empresa"
    };
  }
};
</script>
  <style scoped>
  #header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 1rem;
    background-color: #222;
    
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
    border-radius: 4px;
  }
  
  #logo-url + a {
    color: #ffffff;
    text-decoration: none;
    font-weight: 600;
  }
  </style>