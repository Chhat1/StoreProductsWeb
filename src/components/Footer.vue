<template>
  <footer class="bg-dark text-light pt-5 pb-3 mt-5">

    <div class="container">

      <div class="row g-4">

        <!-- Brand -->
        <div class="col-md-4">
          <h4 class="fw-bold">🛍️ Shopping</h4>
          <p class="text-light">
            Modern shopping website built with Vue 3 + Bootstrap.
            Easy, fast and responsive experience.
          </p>
        </div>

        <!-- Quick Links -->
        <div class="col-md-4">
          <h5 class="fw-semibold mb-3">Quick Links</h5>

          <ul class="list-unstyled">
            <li><a href="#" class="text-decoration-none text-light-50">Home</a></li>
            <li><a href="#" class="text-decoration-none text-light-50">Products</a></li>
            <li><a href="#" class="text-decoration-none text-light-50">Cart</a></li>
            <li><a href="#" class="text-decoration-none text-light-50">Contact</a></li>
          </ul>
        </div>

        <!-- Contact -->
        <div class="col-md-4">
          <h5 class="fw-semibold mb-3">Contact</h5>

          <p class="mb-1">📍 Phnom Penh, Cambodia</p>
          <p class="mb-1">📞 +855 972425545</p>
          <p class="mb-1">📧 support@shopping.com</p>

          <!-- Social -->
          <div class="d-flex justify-content-center d-lg-flex  justify-content-lg-start gap-3 mt-3">
            <a href="#" class="text-light fs-5">🌐</a>
            <a href="#" class="text-light fs-5">📘</a>
            <a href="#" class="text-light fs-5">📸</a>
            <a href="#" class="text-light fs-5">🐦</a>
          </div>

        </div>

      </div>

      <hr class="border-secondary mt-4">

      <!-- Bottom -->
      <div class="text-center text-light small">
        © {{ new Date().getFullYear() }} Shopping. All rights reserved.
      </div>

    </div>

  </footer>
</template>

<script setup>
// no logic needed
</script>

<style scoped>
a.text-light-50 {
  color: rgba(255, 255, 255, 0.7);
  transition: 0.3s;
}

a.text-light-50:hover {
  color: #ffffff;
  padding-left: 4px;
}
</style>