<template>
  <div class="container">

    <!-- NAVBAR -->
    <header class="position-sticky top-0 z-3">
      <nav class="navbar mt-5 rounded-4 navbar-expand-lg navbar-dark bg-dark px-4">

        <a class="navbar-brand fw-bold" href="#">🛍️ Shopping</a>

        <button
          class="btn d-lg-none btn-primary position-relative"
          data-bs-toggle="offcanvas"
          data-bs-target="#checkoutOffcanvas"
        >
          🛒 Cart
          <span class="position-absolute top-0 start-100 translate-middle badge bg-danger">
            {{ cart.length }}
          </span>
        </button>

        <div class="collapse navbar-collapse">

          <ul class="navbar-nav me-auto">
            <li class="nav-item"><a class="nav-link active" href="#">Home</a></li>
            <li class="nav-item"><a class="nav-link" href="#">Products</a></li>
          </ul>

          <button
            class="btn btn-primary position-relative"
            data-bs-toggle="offcanvas"
            data-bs-target="#checkoutOffcanvas"
          >
            🛒 Cart
            <span class="position-absolute top-0 start-100 translate-middle badge bg-danger">
              {{ cart.length }}
            </span>
          </button>

        </div>
      </nav>
    </header>

    <!-- PRODUCTS -->
    <div class="container my-5">
      <h2 class="text-center fw-bold mb-4">🛍️ Products</h2>
      
      <div class="d-flex justify-content-center">
        <img width="100px" v-if="loading" src="https://upload.wikimedia.org/wikipedia/commons/c/c7/Loading_2.gif?_=20170503175831" alt="">
      </div>


      <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-lg-4 g-4">

        <div class="col" v-for="p in products" :key="p.id">

          <div class="card h-100 shadow-sm border-0 rounded-4">

            <div class="bg-light d-flex justify-content-center align-items-center p-3"
              style="height: 200px;">
              <img :src="p.image" class="img-fluid" style="max-height: 100%; object-fit: contain;">
            </div>

            <div class="card-body d-flex flex-column">

              <h6 class="fw-semibold text-truncate">{{ p.title }}</h6>

              <p class="text-success fw-bold fs-5">${{ p.price }}</p>

              <button
                @click="addToCart(p)"
                class="btn btn-primary mt-auto w-100 rounded-pill"
              >
                Add to Cart
              </button>

            </div>

          </div>
        </div>

      </div>
    </div>

    <!-- OFFCANVAS CART -->
    <div class="offcanvas offcanvas-end" id="checkoutOffcanvas">

      <div class="offcanvas-header border-bottom">
        <h5>🧾 CheckOut</h5>
        <button class="btn-close" data-bs-dismiss="offcanvas"></button>
      </div>

      <div class="offcanvas-body d-flex flex-column">

        <div v-if="cart.length === 0" class="text-center text-muted">
          Cart Empty
        </div>

        <div v-for="item in cart" :key="item.id" class="border-bottom pb-3 mb-3">

          <div class="d-flex gap-2 align-items-center">
            <img :src="item.image" width="50" class="rounded">
            <span class="fw-semibold text-truncate">{{ item.title }}</span>
          </div>

          <div class="mt-2">
            <button class="btn btn-sm btn-primary" @click="updateQty(item, -1)">-</button>
            <span class="mx-2">{{ item.qty }}</span>
            <button class="btn btn-sm btn-primary" @click="updateQty(item, 1)">+</button>
          </div>

          <div class="d-flex justify-content-between mt-2">
            <span class="fw-semibold">
              ${{ (item.price * item.qty).toFixed(2) }}
            </span>

            <button class="btn btn-sm btn-outline-danger" @click="removeItem(item.id)">
              ❌
            </button>
          </div>

        </div>

        <div class="mt-auto border-top pt-3 text-center">

          <h5 class="text-danger">
            Total: ${{ total.toFixed(2) }}
          </h5>

          <button
            class="btn btn-success w-100 mt-2"
            data-bs-toggle="modal"
            data-bs-target="#paymentModal"
          >
            Pay Now
          </button>

        </div>

      </div>
    </div>
<!-- PAYMENT MODAL -->
<div class="modal fade" id="paymentModal">
  <div class="modal-dialog modal-dialog-centered modal-lg">

    <div class="modal-content rounded-4 shadow-lg overflow-hidden">

      <!-- HEADER -->
      <div class="modal-header bg-dark text-white">
        <h5 class="mb-0">💳 Secure Checkout</h5>
        <button class="btn-close btn-close-white" data-bs-dismiss="modal"></button>
      </div>

      <div class="modal-body p-4">

        <div class="row g-4">

          <!-- LEFT: ORDER SUMMARY -->
          <div class="col-md-7">

            <h6 class="fw-bold mb-3">🧾 Order Summary</h6>

            <div
              v-for="item in cart"
              :key="item.id"
              class="d-flex align-items-center justify-content-between border-bottom py-2"
            >

              <div class="d-flex align-items-center gap-2">
                <img :src="item.image" width="45" class="rounded">
                <div>
                  <div class="fw-semibold text-truncate" style="max-width: 180px;">
                    {{ item.title }}
                  </div>
                  <small class="text-muted">Qty: {{ item.qty }}</small>
                </div>
              </div>

              <div class="fw-bold text-success">
                ${{ (item.price * item.qty).toFixed(2) }}
              </div>

            </div>

          </div>

          <!-- RIGHT: PAYMENT CARD -->
          <div class="col-md-5">

            <div class="card border-0 shadow-sm rounded-4 p-3">

              <h6 class="fw-bold mb-3">💰 Payment Details</h6>

              <div class="d-flex justify-content-between mb-2">
                <span class="text-muted">Subtotal</span>
                <span>${{ total.toFixed(2) }}</span>
              </div>

              <div class="d-flex justify-content-between mb-2">
                <span class="text-muted">Delivery</span>
                <span>$0.00</span>
              </div>

              <hr>

              <div class="d-flex justify-content-between fs-5 fw-bold">
                <span>Total</span>
                <span class="text-danger">${{ total.toFixed(2) }}</span>
              </div>

              <!-- PAYMENT METHOD -->
              <div class="mt-3">
                <label class="form-label fw-semibold">Payment Method</label>

                <select class="form-select rounded-3 shadow-none">
                  <option>💳 Card Payment</option>
                  <option>📱 QR Pay</option>
                  <option>🏦 Bank Transfer</option>
                </select>
              </div>

              <!-- BUTTON -->
              <button
                class="btn btn-success w-100 mt-4 rounded-pill py-2 fw-bold"
                @click="confirmPayment"
              >
                🔒 Confirm & Pay
              </button>

              <small class="text-muted d-block text-center mt-2">
                Secure payment protected 🔐
              </small>

            </div>

          </div>

        </div>

      </div>

      <!-- FOOTER -->
      <div class="modal-footer bg-light">

        <button class="btn btn-outline-secondary" data-bs-dismiss="modal">
          Cancel
        </button>

      </div>

    </div>

  </div>
</div>


    <!-- SUCCESS MODAL -->
    <div class="modal fade" id="successModal">
      <div class="modal-dialog modal-dialog-centered">

        <div class="modal-content text-center p-4 rounded-4">

          <div class="modal-body">

            <div class="display-1 text-success">✔️</div>

            <h4>Payment Success</h4>

            <p class="text-muted">Order completed 🎉</p>

            <button class="btn btn-success mt-3" @click="closeSuccess">
              OK
            </button>

          </div>

        </div>

      </div>
    </div>

  </div>
</template>

<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const products = ref([])
const cart = ref([])

const loading  = ref(true)

// FETCH
const fetchProduct = async () => {
  const res = await fetch("https://fakestoreapi.com/products")
  products.value = await res.json()
  loading.value = false;
}

// CART
const addToCart = (p) => {
  const exist = cart.value.find(i => i.id === p.id)
  exist ? exist.qty++ : cart.value.push({ ...p, qty: 1 })
}

const updateQty = (item, change) => {
  item.qty += change
  if (item.qty <= 0) removeItem(item.id)
}

const removeItem = (id) => {
  cart.value = cart.value.filter(i => i.id !== id)
}

// TOTAL
const total = computed(() =>
  cart.value.reduce((s, i) => s + i.price * i.qty, 0)
)

const confirmPayment = () => {

  const paymentEl = document.getElementById('paymentModal')
  const paymentModal = bootstrap.Modal.getInstance(paymentEl)

  paymentModal?.hide()

  setTimeout(() => {

    const successEl = document.getElementById('successModal')

    const successModal =
      bootstrap.Modal.getOrCreateInstance(successEl)

    successModal.show()

  }, 500)
}

// SUCCESS CLOSE (FIXED)
const closeSuccess = () => {

  const modalEl = document.getElementById('successModal')
  const modal = bootstrap.Modal.getInstance(modalEl)

  modal?.hide()

  setTimeout(() => {
    document.querySelectorAll('.modal-backdrop').forEach(el => el.remove())
  }, 200)

  cart.value = []
  localStorage.removeItem('cart')
}

// LOCAL STORAGE (optional but recommended)
onMounted(() => {
  fetchProduct()
  cart.value = JSON.parse(localStorage.getItem('cart')) || []
})

watch(cart, () => {
  localStorage.setItem('cart', JSON.stringify(cart.value))
}, { deep: true })

</script>