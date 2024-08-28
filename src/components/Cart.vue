<template>
  <div class="cart">
    <h2>Your Cart</h2>
    <ul>
      <li v-for="item in cart.items" :key="item.id">
        {{ item.name }} - Quantity: {{ item.quantity }} - ${{ item.price * item.quantity }}
        <button @click="removeFromCart(item)">Remove</button>
      </li>
    </ul>
    <div>
      <label for="coupon">Apply Coupon:</label>
      <select id="coupon" @change="applyCoupon($event)">
        <option value="0">None</option>
        <option value="10">10% OFF</option>
        <option value="20">20% OFF</option>
      </select>
    </div>
    <div>Subtotal: ${{ cart.subtotal }}</div>
    <div>Discount: ${{ cart.discount }}</div>
    <div>Total: ${{ cart.total }}</div>
  </div>
</template>

<script>
export default {
  name: 'Cart',
  props: ['cart'],
  emits: ['remove-from-cart', 'apply-coupon'],
  setup(props, { emit }) {
    const removeFromCart = (item) => {
      emit('remove-from-cart', item)
    }

    const applyCoupon = (event) => {
      emit('apply-coupon', Number(event.target.value))
    }

    return {
      removeFromCart,
      applyCoupon
    }
  }
}
</script>

<style scoped>
.cart {
  border: 1px solid #ddd;
  padding: 20px;
  margin-left: 20px;
}
</style>