<script setup>
import { ref, watch, provide, computed } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'

const mokky = 'https://975e4656cc3fbc7b.mokky.dev'
const cart = ref([])
const totalSum = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))

const drawerOpened = ref(false)

const closeDrawer = () => {
    drawerOpened.value = false
}

const openDrawer = () => {
    drawerOpened.value = true
}

const addToCart = async (item) => {
    cart.value.push(item)
    item.isAdded = true
}

const removeFromCart = async (item) => {
    cart.value.splice(cart.value.indexOf(item), 1)
    item.isAdded = false
}

const addRemoveCart = async (item) => {
    if (!item.isAdded) {
        addToCart(item)
    } else {
        removeFromCart(item)
    }
}

const addToFavorite = async (item) => {
    try {
        if (!item.isFavorite) {
            const params = {
                item_id: item.id
            }
            const { data } = await axios.post(`${mokky}/favorites`, params)
            item.isFavorite = true
            item.favoriteId = data.id
        } else {
            const { data } = await axios.delete(`${mokky}/favorites/${item.favoriteId}`)
            item.isFavorite = false
            item.favoriteId = null
        }
    } catch (e) {
        console.log(e)
    }
}

watch(
    cart,
    () => {
        localStorage.setItem('cart', JSON.stringify(cart.value))
    },
    {
        deep: true
    }
)

provide('cart', {
    openDrawer,
    closeDrawer,
    addRemoveCart,
    cart,
    totalSum
})

provide('general', {
    mokky
})
provide('favorite', {
    addToFavorite
})
</script>

<template>
    <Drawer v-if="drawerOpened" />
    <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-10">
        <Header :totalSum="totalSum" @openDrawer="openDrawer" />
        <div class="p-10">
            <router-view></router-view>
        </div>
    </div>
</template>
