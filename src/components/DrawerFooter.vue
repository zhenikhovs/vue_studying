<script setup>
import axios from 'axios'

import { inject, computed, ref } from 'vue'
const { totalSum, cart } = inject('cart')
const { mokky } = inject('general')
const { orderNum } = inject('order')

const isCreatingOrder = ref(false)

const buttonDisabled = computed(() => {
    return isCreatingOrder.value ? true : totalSum.value ? false : true
})

const vatSum = computed(() => Math.round(totalSum.value * 0.05))

const createOrder = async () => {
    try {
        isCreatingOrder.value = true
        const { data } = await axios.post(`${mokky}/orders`, {
            items: cart.value,
            totalSum: totalSum.value
        })

        cart.value = []

        orderNum.value = data.id
        return data
    } catch (e) {
        console.log(e)
    } finally {
        isCreatingOrder.value = false
    }
}
</script>
<template>
    <div class="flex flex-col gap-4 mt-10 sticky bottom-0 bg-white py-10">
        <div class="flex gap2">
            <span>Итого:</span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>{{ totalSum }} руб.</b>
        </div>
        <div class="flex gap2">
            <span>Налог 5%:</span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>{{ vatSum }} руб.</b>
        </div>
        <button
            :disabled="buttonDisabled"
            @click="createOrder"
            class="mt-4 bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 cursor-pointer hover:bg-lime-600 transition active:bg-lime-700"
        >
            Оформить заказ
        </button>
    </div>
</template>
