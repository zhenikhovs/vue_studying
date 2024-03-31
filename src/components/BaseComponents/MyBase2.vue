<script setup>
const props = defineProps({
    title: String,
    price: Number,
    isAdded: Boolean
})

console.log(props)
</script>

<template>
    <div class="product">
        <h2 class="text-blue-800">Товар: {{ title }}</h2>
        <h3>Цена: {{ price }}</h3>
        <h3>Добавлен: {{ isAdded ? 'Да' : 'Нет' }}</h3>
        <slot></slot>
    </div>
</template>

<style scoped>
.product {
    border: 1px solid blue;
    padding: 10px;
    margin-bottom: 10px;
    width: max-content;
}
</style>
