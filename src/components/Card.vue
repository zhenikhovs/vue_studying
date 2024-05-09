<script setup>
defineProps({
    id: Number,
    title: String,
    imageUrl: String,
    price: Number,
    isFavorite: Boolean,
    isAdded: Boolean,
    onClickAdd: Function,
    onClickFavorite: Function
})
</script>

<template>
    <div
        class="relative flex flex-col bg-white border border-slate-100 rounded-3xl p-8 hover:-translate-y-2 hover:shadow-xl transition gap-y-2"
    >
        <div class="absolute top-8 left-8">
            <img
                class="cursor-pointer"
                @click="onClickFavorite"
                :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
                alt="like 1"
            />
        </div>
        <img class="w-[200px] cursor-pointer" :src="imageUrl" alt="like 2" />

        <div class="cursor-pointer">{{ title }}</div>

        <div class="flex justify-between mt-auto">
            <div class="flex flex-col">
                <span class="text-slate-400">Цена:</span>
                <b>{{ price }} руб.</b>
            </div>
            <img
                class="cursor-pointer"
                @click="onClickAdd"
                :src="isAdded ? '/checked.svg' : '/plus.svg'"
                alt="Plus"
            />
        </div>
    </div>
</template>
