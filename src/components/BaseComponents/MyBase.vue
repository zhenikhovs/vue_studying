<script setup>
//<script>
// export default {
//   data() {
//     return {
//       count: 0
//     }
//   },

//   methods: {
//     increment() {
//       this.count++
//     },

//     deccrement() {
//       this.count--
//     }
//   }
// }

import { ref } from 'vue'
const count = ref(0)

function increment() {
    count.value++
    console.log(count)
}
function deccrement() {
    count.value--
    console.log(count)
}

function updateCount(event) {
    count.value = +event.target.value
}
</script>

<template>
    <h2>{{ count }}</h2>
    <div>
        <button @click="increment">+</button>
        <button @click="deccrement">-</button>
    </div>
    <input @input="updateCount" type="number" />
    <input v-model="count" type="number" />
    <input @keyup.enter.ctrl="increment" />
</template>
