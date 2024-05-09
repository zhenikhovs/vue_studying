<script setup>
defineProps({
    imageUrl: String,
    title: String,
    description: String
})
</script>

<template>
    <div class="mt-20 flex gap-4 flex-col items-center text-center h-full justify-center">
        <img class="h-[70px]; w-[70px]" :src="imageUrl" alt="Basket-icon" />
        <div class="text-2xl font-medium">{{ title }}</div>
        <div class="text-gray-400">{{ description }}</div>
    </div>
</template>
