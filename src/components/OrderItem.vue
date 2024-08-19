<script setup>
import { defineProps } from 'vue'
defineProps(['order'])
</script>
<template>
  <div class="card-body">
    <div class="card-title">
      <h3>訂單</h3>
    </div>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">品項</th>
          <th scope="col">數量</th>
          <th scope="col">小計</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="order in order.cart" :key="order.id">
          <td>{{ order.name }}</td>
          <td>{{ order.amount }}</td>
          <td>{{ order.price * order.amount }}</td>
        </tr>
      </tbody>
    </table>
    <div class="text-end mb-3">
      <div>備註：{{ order.description }}</div>
      <br />
      <h4>
        總計：<span>${{ order.total }}</span>
      </h4>
    </div>
  </div>
</template>
