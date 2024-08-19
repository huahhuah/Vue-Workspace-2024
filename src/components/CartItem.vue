<script setup>
import { defineProps, defineEmits } from 'vue'
defineProps(['cart'])
const emit = defineEmits(['deleteCart', 'updateCart'])
</script>

<template>
  <tr v-for="carts in cart" :key="carts.id">
    <td>
      <button @click="emit('deleteCart', carts.id)" class="btn btn-secondary btn-sm" type="button">
        x
      </button>
    </td>
    <td>{{ carts.name }}</td>
    <td>
      <small>{{ carts.description }}</small>
    </td>
    <td>
      <select class="form-select" v-model="carts.amount" @change="emit('updateCart', carts)">
        <option v-for="n in 10" :key="n" :value="n">{{ n }}</option>
      </select>
    </td>
    <td>{{ carts.price }}</td>
    <td>{{ carts.price * carts.amount }}</td>
  </tr>
</template>
