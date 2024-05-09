<script setup>
import { inject } from 'vue'
const onChangeSelect = inject('onChangeSelect')
const onChangeSearch = inject('onChangeSearch')
</script>

<template>
    <div class="flex gap-4">
        <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
            <option value="title">По названию</option>
            <option value="price">По возрастанию цены</option>
            <option value="-price">По убыванию цены</option>
        </select>
        <div class="relative">
            <img src="/search.svg" class="absolute left-4 top-3" alt="" />
            <input
                @input="onChangeSearch"
                class="border rounded-md py-2 pl-10 pr-4 outline-none focus:border-gray-400"
                placeholder="Поиск"
            />
        </div>
    </div>
</template>
