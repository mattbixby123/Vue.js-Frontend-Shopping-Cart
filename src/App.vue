<template>
  <div class="app">
    <h1>Shopping Cart Checkout</h1>
    <div class="layout-row">
      <ProductList :products="products" :cart="cart" @add-to-cart="addToCart" @remove-from-cart="removeFromCart" />
      <Cart :cart="cart" @remove-from-cart="removeFromCart" @apply-coupon="applyCoupon" />
    </div>
  </div>
</template>

<script>
import { ref, reactive } from 'vue'
import ProductList from './components/ProductList.vue'
import Cart from './components/Cart.vue'

export default {
  name: 'App',
  components: {
    ProductList,
    Cart
  },
  setup() {
    const products = ref([
      { id: 1, name: 'Cap', price: 10, image: 'cap.png' },
      { id: 2, name: 'HandBag', price: 30, image: 'handbag.png' },
      { id: 3, name: 'Shirt', price: 30, image: 'shirt.png' },
      { id: 4, name: 'Shoe', price: 50, image: 'shoe.png' },
      { id: 5, name: 'Pant', price: 40, image: 'pant.png' },
      { id: 6, name: 'Slipper', price: 20, image: 'slipper.png' }
    ])

    const cart = reactive({
      items: [],
      subtotal: 0,
      discount: 0,
      total: 0
    })

    const addToCart = (product) => {
      const existingItem = cart.items.find(item => item.id === product.id)
      if (existingItem) {
        existingItem.quantity++
      } else {
        cart.items.push({ ...product, quantity: 1 })
      }
      updateCartTotals()
    }

    const removeFromCart = (product) => {
      const index = cart.items.findIndex(item => item.id === product.id)
      if (index !== -1) {
        if (cart.items[index].quantity > 1) {
          cart.items[index].quantity--
        } else {
          cart.items.splice(index, 1)
        }
        updateCartTotals()
      }
    }

    const updateCartTotals = () => {
      cart.subtotal = cart.items.reduce((total, item) => total + item.price * item.quantity, 0)
      cart.total = cart.subtotal - cart.discount
    }

    const applyCoupon = (couponPercentage) => {
      cart.discount = (cart.subtotal * couponPercentage) / 100
      updateCartTotals()
    }

    return {
      products,
      cart,
      addToCart,
      removeFromCart,
      applyCoupon
    }
  }
}
</script>

<style>
.layout-row {
  display: flex;
}
</style>