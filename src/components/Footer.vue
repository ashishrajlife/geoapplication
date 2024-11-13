<template>
  <footer class="footer">
    <div class="container text-center">
      <div class="logos">
        <img src="../assets/images/facebook.png" alt="Facebook">
        <img src="../assets/images/instagram.png" alt="Instagram">
        <img src="../assets/images/linkedin.png" alt="LinkedIn">
        <img src="../assets/images/twitter.png" alt="Twitter">
        <img src="../assets/images/youtube.png" alt="YouTube">
      </div>
      <p class="footer-description">© 2024 All Rights Reserved </p>
    </div>
  </footer>
</template>
  
  <script>
  export default {
    name: 'FooterView',
  };
</script>

<style scoped>
.footer {
  background: linear-gradient(45deg, #ffdd57, #6c757d, #f8d347);
  padding: 20px 0;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
  animation: fadeIn 1.2s ease;
  overflow: hidden;
}

.container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.footer-description {
  color: #333;
  font-weight: 600;
  font-size: 1.1rem;
}

.logos {
  display: flex;
  gap: 20px;
}

.logos img {
  width: 30px;
  height: 30px;
  transition: transform 0.3s ease, filter 0.3s ease;
}

.logos img:hover {
  transform: scale(1.1);
  filter: grayscale(0%);
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>