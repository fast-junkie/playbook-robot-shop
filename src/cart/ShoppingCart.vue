<template>
  <div class="container">
    <h1>Your Cart</h1>
    <EmptyCart v-if="cart.length === 0" />
    <ul class="cart" v-if="cart.length > 0">
      <li class="cart-item" v-for="(product, index) in cart" :key="index">
        <ProductInfo :product="product">
          <BaseButton @click="removeFromCart(product)">Remove</BaseButton>
        </ProductInfo>
      </li>
    </ul>
    <div v-if="cart.length > 0" class="total">Total: {{ toCurrency(cartTotal) }}</div>
  </div>
</template>

<script setup lang="ts">
import { defineAsyncComponent } from 'vue'
import { storeToRefs } from 'pinia'
import { useCartStore } from '@/stores/cart'
import { toCurrency } from '@/shared/formatters'

const ErrorFallback = defineAsyncComponent(() => import('@/shared/ErrorFallback.vue'))
const LoadingMessage = defineAsyncComponent(() => import('@/shared/LoadingMessage.vue'))
const EmptyCart = defineAsyncComponent(() => import('./EmptyCart.vue'))
const ProductInfo = defineAsyncComponent({
  loader: () => new Promise((resolve) => {
    setTimeout(() => import('@/catalog/product-info/ProductInfo.vue').then((module) => resolve(module)), 2e3)
  }),
  loadingComponent: LoadingMessage,
  delay: 2e2,
  timeout: 5e3,
  errorComponent: ErrorFallback,
})
const cartStore = useCartStore()
const { cart, cartTotal } = storeToRefs(cartStore)
const { removeFromCart } = cartStore

</script>

<style scoped>
.cart {
  margin: 0 100px;
  border-top: 2px solid #999;
}

.cart-item {
  border-bottom: 2px solid #999;
}

.total {
  margin: 25px 175px 0 0;
  align-self: flex-end;
  font-size: 25px;
  text-align: right;
}

.empty-cart {
  margin-top: 20px;
  font-size: 20px;
}
</style>
