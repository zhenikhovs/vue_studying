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
const emit = defineEmits(['removeFromCart'])
</script>

<template>
    <div class="w-full items-center border border-slate-100 p-4 rounded-xl flex gap-4">
        <img :src="imageUrl" class="w-16 h-16" :alt="title" />

        <div class="flex flex-col w-full">
            <span class="w-full">{{ title }}</span>
            <div class="flex justify-between mt-2">
                <b>{{ price }} руб. </b>
                <img
                    @click="() => emit('removeFromCart')"
                    class="opacity-40 hover:opacity-100 transition cursor-pointer"
                    src="/close.svg"
                />
            </div>
        </div>
    </div>
</template>
