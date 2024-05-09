<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'
const { cart, addRemoveCart } = inject('cart')

const onClickRemove = (item) => {
    addRemoveCart(item)
}
</script>

<template>
    <div class="flex flex-col gap-2 mt-5" v-auto-animate>
        <CartItem
            v-for="item in cart"
            :key="item.id"
            :title="item.title"
            :price="item.price"
            :imageUrl="item.imageUrl"
            @removeFromCart="onClickRemove(item)"
        />
    </div>
</template>
