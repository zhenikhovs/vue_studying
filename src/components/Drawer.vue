<script setup>
import { onMounted, onUnmounted, inject, ref, computed, provide } from 'vue'

import DrawerHead from './DrawerHead.vue'
import DrawerFooter from './DrawerFooter.vue'
import CartItemList from './CartItemList.vue'
import InfoComponent from './InfoComponent.vue'
const { closeDrawer, cart } = inject('cart')

const drawerClick = () => {
    closeDrawer()
}

const handleKeyDownEsc = (event) => {
    if (event.key === 'Escape') {
        closeDrawer()
    }
}

onMounted(() => {
    window.addEventListener('keydown', handleKeyDownEsc)
})

onUnmounted(() => {
    window.removeEventListener('keydown', handleKeyDownEsc)
})

const orderNum = ref(null)

provide('order', {
    orderNum
})
</script>

<template>
    <div
        @click="drawerClick"
        class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"
    ></div>

    <div
        class="bg-white w-96 h-full overflow-y-auto fixed right-0 top-0 z-20 p-8 pb-0 flex flex-col"
    >
        <DrawerHead />
        <div v-if="!cart.length || orderNum">
            <InfoComponent
                v-if="!cart.length && !orderNum"
                image-url="/package-icon.png"
                title="Корзина пуста"
                description="Добавьте товары в корзину, чтобы сделать заказ"
            />

            <InfoComponent
                v-if="orderNum"
                image-url="/order-success-icon.png"
                title="Заказ оформлен"
                :description="`Ваш заказ #${orderNum} скоро будет передан в доставку`"
            />
        </div>

        <div v-else>
            <CartItemList />
            <DrawerFooter :orderNum="orderNum" />
        </div>
    </div>
</template>
