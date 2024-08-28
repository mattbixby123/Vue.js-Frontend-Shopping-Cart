<template>
  <div class="product-list">
    <div v-for="product in products" :key="product.id" class="product-item">
      <img :src="'/images/' + product.image" :alt="product.name">
      <h3>{{ product.name }}</h3>
      <p>${{ product.price }}</p>
      <button @click="addToCart(product)" v-if="!isInCart(product)">Add to Cart</button>
      <button @click="removeFromCart(product)" v-else>Remove from Cart</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProductList',
  props: ['products', 'cart'],
  emits: ['add-to-cart', 'remove-from-cart'],
  setup(props, { emit }) {
    const isInCart = (product) => {
      return props.cart.items.some(item => item.id === product.id)
    }

    const addToCart = (product) => {
      emit('add-to-cart', product)
    }

    const removeFromCart = (product) => {
      emit('remove-from-cart', product)
    }

    return {
      isInCart,
      addToCart,
      removeFromCart
    }
  }
}
</script>

<style scoped>
.product-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.product-item {
  border: 1px solid #ddd;
  padding: 10px;
  width: 200px;
}

img {
  max-width: 100%;
}
</style>