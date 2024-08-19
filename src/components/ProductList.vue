<script setup>
import { defineProps, defineEmits } from 'vue'

defineProps(['data'])
const emit = defineEmits(['addCart'])
</script>

<template>
  <a
    href="#"
    class="list-group-item list-group-item-action"
    v-for="data in data"
    :key="data.id"
    @click.prevent="emit('addCart', data)"
  >
    <div class="d-flex w-100 justify-content-between">
      <h5 class="mb-1">
        {{ data.name }}
        <small>${{ data.price }}</small>
      </h5>
    </div>
    <p class="mb-1">{{ data.description }}</p>
  </a>
</template>
