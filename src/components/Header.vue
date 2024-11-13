<template>
    <header class="app-header">
      <nav class="nav-links">
        <router-link to="/Home" class="nav-link">Home</router-link>
        <router-link to="/adddata" class="nav-link">+ Geo Data</router-link>
        <button @click="logout" class="nav-link">Logout</button>
      </nav>
    </header>
  </template>
  
  <script>
    import { toast } from 'vue3-toastify';
    import 'vue3-toastify/dist/index.css';
  export default {
    name: "AppHeader",
    methods: {
      logout() {
        localStorage.removeItem("user-info");
        // this.$router.push({ name: "Login" });
        toast.error('Logged out !',{ autoClose:3500});

        setTimeout(() => {
    this.$router.push({ name: "Login" });
  }, 3500);

      }
    }
  };
  </script>
  
  <style scoped>
  .app-header {
    background: linear-gradient(45deg, #ffdd57, #6c757d, #f8d347); /* Yellow and gray gradient */
    padding: 10px 10px;
    display: flex;
    justify-content: center;
    align-items: center;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
    animation: fadeIn 1.2s ease;
  }
  
  @keyframes fadeIn {
    from {
      opacity: 0;
      transform: translateY(-20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  
  .nav-links {
    display: flex;
    gap: 30px;
  }
  
  .nav-link {
    color: black;
    font-size: 1.1rem;
    font-weight: 500;
    text-decoration: none;
    padding: 8px 12px;
    border-radius: 8px;
    transition: all 0.3s ease;
    position: relative;
    overflow: hidden;
  }
  
  .nav-link::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.2);
    transition: all 0.4s ease;
  }
  
  .nav-link:hover::before {
    left: 0;
  }
  
  .nav-link:hover {
    color: yellow;
    transform: translateY(-2px);
  }

  </style>
  
  