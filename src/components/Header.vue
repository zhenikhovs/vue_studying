<script setup>
defineProps({
    totalSum: Number
})
const emit = defineEmits(['openDrawer'])
</script>
<template>
    <header class="flex justify-between border-b border-slate-100 px-10 py-8">
        <router-link to="/">
            <div class="flex items-center gap-4">
                <img src="/logo.png" alt="Logo" class="w-10 h-10" />
                <div class="flex flex-col">
                    <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
                    <div class="text-slate-400">Магазин лучших кроссовок</div>
                </div>
            </div>
        </router-link>

        <ul class="flex items-center gap-10">
            <li
                @click="
                    () => {
                        emit('openDrawer')
                    }
                "
                class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black"
            >
                <img src="/cart.svg" alt="Cart" />
                <b>{{ totalSum }} руб.</b>
            </li>
            <router-link to="/favorites">
                <li class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black">
                    <img src="/heart.svg" alt="Cart" />
                    <span>Избранное</span>
                </li>
            </router-link>

            <li class="flex items-cente cursor-pointer gap-3 text-gray-500 hover:text-black">
                <img src="/profile.svg" alt="Cart" />
                <span>Профиль</span>
            </li>
        </ul>
    </header>
</template>
