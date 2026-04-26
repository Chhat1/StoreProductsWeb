<template>
  <div class="container">

    <!-- NAVBAR -->
    <header class="position-sticky top-0 z-3">
      <nav class="navbar mt-5 rounded-4 navbar-expand-lg navbar-dark bg-dark px-4">

        <a class="navbar-brand fw-bold" href="#">🛍️ Shopping</a>


        <button
            class="btn d-lg-none btn-primary position-relative"
            data-bs-toggle="offcanvas"
            data-bs-target="#checkoutOffcanvas">

            🛒 Cart

            <span class="position-absolute top-0 start-100 translate-middle badge bg-danger">
              {{ cart.length }}
            </span>

          </button>
        

        <button class="navbar-toggler" type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarNav">
          <span class="navbar-toggler-icon"></span>
        </button>


        


        <div class="collapse navbar-collapse" id="navbarNav">

          <ul class="navbar-nav me-auto">
            <li class="nav-item">
              <a class="nav-link active" href="#">Home</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#">Products</a>
            </li>
          </ul>

          <button
            class="btn btn-primary position-relative"
            data-bs-toggle="offcanvas"
            data-bs-target="#checkoutOffcanvas">

            🛒 Cart

            <span class="position-absolute top-0 start-100 translate-middle badge bg-danger">
              {{ cart.length }}
            </span>

          </button>
        </div>
      </nav>
    </header>

    <!-- PRODUCTS -->
    <div id="products" class="container my-5">
      <h2 class="text-center fw-bold mb-4">🛍️ Products</h2>

      <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-lg-4 g-4">

        <div class="col" v-for="p in products" :key="p.id">

          <div class="card h-100 shadow-sm border-0 rounded-4">

            <div class="bg-light d-flex justify-content-center align-items-center p-3 rounded-top-4"
              style="height: 200px;">
              <img :src="p.image" class="img-fluid" style="max-height: 100%; object-fit: contain;">
            </div>

            <div class="card-body d-flex flex-column">

              <h6 class="fw-semibold text-truncate">
                {{ p.title }}
              </h6>

              <p class="text-success fw-bold fs-5 mb-3">
                ${{ p.price }}
              </p>

              <button
                @click="addToCart(p)"
                class="btn btn-primary mt-auto w-100 rounded-pill">
                Add to Cart
              </button>

            </div>

          </div>
        </div>

      </div>
    </div>

    <!-- OFFCANVAS CART -->
    <div class="offcanvas offcanvas-end" tabindex="-1" id="checkoutOffcanvas">

      <div class="offcanvas-header border-bottom">
        <h5 class="offcanvas-title">🧾 CheckOut</h5>
        <button type="button" class="btn-close" data-bs-dismiss="offcanvas"></button>
      </div>

      <div class="offcanvas-body d-flex flex-column">

        <div v-if="cart.length === 0" class="text-center text-muted">
          Cart Empty
        </div>

        <div v-for="item in cart" :key="item.id" class="border-bottom pb-3 mb-3">

          <div class="d-flex align-items-center gap-2">
            <img :src="item.image" width="50" class="rounded">
            <span class="fw-semibold text-truncate">{{ item.title }}</span>
          </div>

          <div class="mt-2">
            <button class="btn btn-sm btn-primary" @click="updateQty(item, -1)">-</button>
            <span class="mx-2">{{ item.qty }}</span>
            <button class="btn btn-sm btn-primary" @click="updateQty(item, 1)">+</button>
          </div>

          <div class="d-flex justify-content-between align-items-center mt-2">
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
<div class="modal fade" id="paymentModal" tabindex="-1">
  <div class="modal-dialog modal-dialog-centered">

    <div class="modal-content rounded-4">

      <!-- Header -->
      <div class="modal-header">
        <h5 class="modal-title">💳 Payment Method</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
      </div>

      <!-- Body -->
      <div class="modal-body text-center">

        <!-- Tabs -->
        <ul class="nav nav-pills justify-content-center mb-3">
          <li class="nav-item">
            <button class="nav-link active" data-bs-toggle="pill" data-bs-target="#qr">
              QR Code
            </button>
          </li>

          <li class="nav-item">
            <button class="nav-link" data-bs-toggle="pill" data-bs-target="#visa">
              Visa Card
            </button>
          </li>
        </ul>

        <div class="tab-content">

          <!-- QR CODE -->
          <div class="tab-pane fade show active" id="qr">
            <p class="text-muted">Scan to pay</p>

            <img
              src="https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=Pay%20Now%20Order"
              class="img-fluid rounded"
              alt="QR Code"
            />

            <h5 class="text-danger mt-3">
            Total: ${{ total.toFixed(2) }}
          </h5>

          </div>

          <!-- VISA CARD -->
          <div class="tab-pane fade" id="visa">

            <div class="card bg-dark text-white p-3 rounded-4 text-start">

              <div class="d-flex justify-content-between">
                <span>💳 VISA</span>
                <span>Bank</span>
              </div>

              <h5 class="mt-3">**** **** **** 1234</h5>

              <div class="d-flex justify-content-between mt-3">
                <small>Card Holder</small>
                <small>EXPIRE 12/30</small>
              </div>

              <strong class="mt-2">CHHAT RO</strong>

            </div>

          </div>

        </div>

      </div>

      <!-- Footer -->
      <div class="modal-footer">
        <button class="btn btn-secondary" data-bs-dismiss="modal">
          Close
        </button>

        <button
            class="btn btn-primary"
            @click="confirmPayment"
             data-bs-toggle="modal"
            data-bs-target="#successModal" >
            Confirm Payment
            </button>
      </div>

    </div>
  </div>
</div>


<!-- SUCCESS MODAL -->
<div class="modal fade" id="successModal" tabindex="-1">
  <div class="modal-dialog modal-dialog-centered">

    <div class="modal-content rounded-4 text-center p-4">

      <div class="modal-body">

        <div class="display-1 text-success">✔️</div>

        <h4 class="fw-bold mt-3">Payment Success</h4>

        <p class="text-muted">
          Your order has been successfully paid 🎉
        </p>

        <button
          class="btn btn-success mt-3"
          @click="closeSuccess"
        >
          OK
        </button>


      </div>

    </div>

  </div>
</div>

  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue'

const products = ref([])
const cart = ref([])

// FETCH PRODUCTS
const fetchProduct = async () => {
  const res = await fetch("https://fakestoreapi.com/products")
  products.value = await res.json()
}

// ADD TO CART
const addToCart = (p) => {
  const exist = cart.value.find(i => i.id === p.id)
  if (exist) {
    exist.qty++
  } else {
    cart.value.push({ ...p, qty: 1 })
  }
}

// UPDATE QTY
const updateQty = (item, change) => {
  item.qty += change
  if (item.qty <= 0) {
    removeItem(item.id)
  }
}

// REMOVE ITEM
const removeItem = (id) => {
  cart.value = cart.value.filter(i => i.id !== id)
}

// TOTAL (FIXED: return NUMBER ONLY)
const total = computed(() => {
  return cart.value.reduce((sum, i) => sum + i.price * i.qty, 0)
})

// ON MOUNT
onMounted(() => {
  fetchProduct()
})

const closeSuccess = () => {
  const modalEl = document.getElementById('successModal')
  const modal = bootstrap.Modal.getInstance(modalEl)

  modal?.hide()

  document.querySelectorAll('.modal-backdrop').forEach(el => el.remove())

  // clear cart
  cart.value = []

  // clear storage
  localStorage.removeItem('cart')
}

</script>