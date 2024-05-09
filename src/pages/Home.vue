<script setup>
import axios from 'axios'
import Card from '../components/Card.vue'
import CardListFilters from '../components/CardListFilters.vue'
import debounce from 'lodash.debounce'
import { inject, provide, reactive, watch, ref, onMounted } from 'vue'
const { addRemoveCart, cart } = inject('cart')
const { addToFavorite } = inject('favorite')
const { mokky } = inject('general')

const items = ref([])

const fetchFavorites = async () => {
    try {
        const { data: favorites } = await axios.get(`${mokky}/favorites`)
        items.value = items.value.map((item) => {
            const favorite = favorites.find((favorite) => favorite.item_id === item.id)
            if (!favorite) {
                return item
            }
            return {
                ...item,
                isFavorite: true,
                favoriteId: favorite.id
            }
        })
    } catch (e) {
        console.log(e)
    }
}

const fetchItems = async () => {
    try {
        const params = {
            sortBy: filters.sortBy
        }

        if (filters.searchQuery) {
            params.title = `*${filters.searchQuery}*`
        }

        const { data } = await axios.get(`${mokky}/items`, {
            params
        })
        items.value = data.map((item) => ({
            ...item,
            isFavorite: false,
            favoriteId: null,
            isAdded: false
        }))
    } catch (e) {
        console.log(e)
    }
}

onMounted(async () => {
    const LSCart = localStorage.getItem('cart')
    if (LSCart) {
        cart.value = JSON.parse(LSCart)
    }

    await fetchItems()
    await fetchFavorites()

    items.value = items.value.map((item) => ({
        ...item,
        isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
    }))
})

const filters = reactive({
    sortBy: 'title',
    searchQuery: ''
})
watch(filters, fetchItems)
watch(cart, () => {
    items.value = items.value.map((item) => ({
        ...item,
        isAdded: false
    }))
})

const onChangeSelect = (event) => {
    filters.sortBy = event.target.value
}

const onChangeSearch = debounce((event) => {
    filters.searchQuery = event.target.value
}, 300)

provide('onChangeSelect', onChangeSelect)
provide('onChangeSearch', onChangeSearch)
</script>

<template>
    <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
        <CardListFilters />
    </div>
    <div class="grid grid-cols-4 gap-5 mt-5" v-auto-animate>
        <Card
            v-for="item in items"
            :key="item.id"
            :id="item.id"
            :title="item.title"
            :image-url="item.imageUrl"
            :price="item.price"
            :isFavorite="item.isFavorite"
            :isAdded="item.isAdded"
            :onClickFavorite="() => addToFavorite(item)"
            :onClickAdd="() => addRemoveCart(item)"
        />
    </div>
</template>
