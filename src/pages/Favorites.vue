<script setup>
import axios from 'axios'
import Card from '../components/Card.vue'
import CardListFilters from '../components/CardListFilters.vue'
import { inject, ref, onMounted } from 'vue'
const { addRemoveCart, cart } = inject('cart')
const { addToFavorite } = inject('favorite')
const { mokky } = inject('general')

const favorites = ref([])

const fetchFavorites = async () => {
    const LSCart = localStorage.getItem('cart')
    if (LSCart) {
        cart.value = JSON.parse(LSCart)
    }

    try {
        const { data } = await axios.get(`${mokky}/favorites?_relations=items`)
        favorites.value = data.map((favorite) => ({
            ...favorite.item,
            isFavorite: true,
            favoriteId: favorite.id,
            isAdded: cart.value.some((cartItem) => cartItem.id === favorite.item.id)
        }))
    } catch (e) {
        console.log(e)
    }
}

const onClickFavorite = (favorite) => {
    addToFavorite(favorite)
    favorites.value.splice(favorites.value.indexOf(favorite), 1)
}
onMounted(() => {
    fetchFavorites()
})
</script>
<template>
    <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Избранное</h2>
        <!-- <CardListFilters /> -->
    </div>
    <div class="grid grid-cols-4 gap-5 mt-5" v-auto-animate>
        <Card
            v-for="favorite in favorites"
            :key="favorite.id"
            :id="favorite.id"
            :title="favorite.title"
            :image-url="favorite.imageUrl"
            :price="favorite.price"
            :isFavorite="favorite.isFavorite"
            :isAdded="favorite.isAdded"
            :onClickFavorite="() => onClickFavorite(favorite)"
            :onClickAdd="() => addRemoveCart(favorite)"
        />
    </div>
</template>
