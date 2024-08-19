<script setup>
import { ref } from 'vue'
const productList = ref([
  {
    id: 1,
    name: '珍珠奶茶',
    description: '香濃奶茶搭配QQ珍珠',
    price: 50,
    stock: 20
  },
  {
    id: 2,
    name: '冬瓜檸檬',
    description: '清新冬瓜配上新鮮檸檬',
    price: 45,
    stock: 18
  },
  {
    id: 3,
    name: '翡翠檸檬',
    description: '綠茶與檸檬的完美結合',
    price: 55,
    stock: 34
  },
  {
    id: 4,
    name: '四季春茶',
    description: '香醇四季春茶，回甘無比',
    price: 45,
    stock: 10
  },
  {
    id: 5,
    name: '阿薩姆奶茶',
    description: '阿薩姆紅茶搭配香醇鮮奶',
    price: 50,
    stock: 25
  },
  {
    id: 6,
    name: '檸檬冰茶',
    description: '檸檬與冰茶的清新組合',
    price: 45,
    stock: 20
  },
  {
    id: 7,
    name: '芒果綠茶',
    description: '芒果與綠茶的獨特風味',
    price: 55,
    stock: 18
  },
  {
    id: 8,
    name: '抹茶拿鐵',
    description: '抹茶與鮮奶的絕配',
    price: 60,
    stock: 20
  }
])
const tempEdit = ref({
  id: '',
  name: '',
  description: '',
  price: '',
  stock: ''
})
const addStock = (item) => {
  item.stock++
}
const minusStock = (item) => {
  if (item.stock > 0) {
    item.stock--
  }
}
const prepareEdit = (item) => {
  tempEdit.value = { ...item }
  console.log(tempEdit.value)
}
const confirm = () => {
  const index = productList.value.findIndex((item) => item.id === tempEdit.value.id)
  productList.value[index] = tempEdit.value
  tempEdit.value = {}
}
</script>

<template>
  <h3>2024 Vue 前端新手營 week 1 homework</h3>
  <br />
  <table>
    <thead>
      <tr>
        <th scope="col">品項</th>
        <th scope="col">描述</th>
        <th scope="col">價格</th>
        <th></th>
        <th scope="col">庫存</th>
        <th></th>
        <th></th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in productList" v-bind:key="item.id">
        <td>{{ item.name }}</td>
        <td>
          <small>{{ item.description }}</small>
        </td>
        <td>{{ item.price }}</td>
        <td><button type="button" @click="minusStock(item)">-</button></td>
        <td>
          {{ item.stock }}
        </td>
        <td><button type="button" @click="addStock(item)">+</button></td>
        <td>
          <button type="button" @click="prepareEdit(item)">編輯</button>
        </td>
      </tr>
    </tbody>
  </table>
  <div v-if="tempEdit.id">
    <h3>修改產品資料</h3>
    <hr />
    <label for="name">品項</label>
    <input id="name" type="text" v-model="tempEdit.name" />
    <br />
    <label for="description">描述</label>
    <input id="description" type="text" v-model="tempEdit.description" />
    <br />
    <label for="price">價格</label>
    <input id="price" type="number" v-model="tempEdit.price" />
    <br />
    <label for="stock">數量</label>
    <input id="stock" type="number" v-model="tempEdit.stock" />
    <br />
    <button type="button" @click="confirm">確認</button>
    <button type="button" @click="tempEdit = {}">取消</button>
  </div>
</template>

<style scoped></style>
