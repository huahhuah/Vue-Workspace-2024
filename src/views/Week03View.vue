<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-4">
        <div class="list-group">
          <ProductList :data="data" @add-cart="addCart" />
        </div>
      </div>
      <div class="col-md-8">
        <div v-if="cart.length === 0" class="alert alert-primary text-center" role="alert">
          請選擇商品
        </div>
        <div v-else>
          <table class="table">
            <thead>
              <tr>
                <th scope="col" width="50">操作</th>
                <th scope="col">品項</th>
                <th scope="col">描述</th>
                <th scope="col" width="90">數量</th>
                <th scope="col">單價</th>
                <th scope="col">小計</th>
              </tr>
            </thead>
            <tbody>
              <CartItem
                :cart="cart"
                @deleteCart="deleteCart"
                @updateCart="updateCart"
                @itemSum="itemSum"
              />
            </tbody>
          </table>
          <div class="text-end mb-3">
            <h4>
              總計：<span>${{ total }}</span>
            </h4>
            <textarea
              class="form-control mb-3"
              rows="3"
              placeholder="備註"
              v-model="description"
            ></textarea>
            <div class="text-end">
              <button @click.prevent="createOrder" class="btn btn-primary">送出</button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <hr />
    <div class="row justify-content-center">
      <div class="col-md-8">
        <div v-if="!order.id" class="alert alert-primary text-center" role="alert">
          尚未建立訂單
        </div>
        <div v-else class="card">
          <OrderItem :order="order" @itemSum="itemSum" />
        </div>
      </div>
    </div>
    <br />
  </div>
</template>
<script setup>
import { ref, computed } from 'vue'
import ProductList from '@/components/ProductList.vue'
import CartItem from '@/components/CartItem.vue'
import OrderItem from '@/components/OrderItem.vue'

const data = ref([
  {
    id: 1,
    name: '珍珠奶茶',
    description: '香濃奶茶搭配QQ珍珠',
    price: 50
  },
  {
    id: 2,
    name: '冬瓜檸檬',
    description: '清新冬瓜配上新鮮檸檬',
    price: 45
  },
  {
    id: 3,
    name: '翡翠檸檬',
    description: '綠茶與檸檬的完美結合',
    price: 55
  },
  {
    id: 4,
    name: '四季春茶',
    description: '香醇四季春茶，回甘無比',
    price: 45
  },
  {
    id: 5,
    name: '阿薩姆奶茶',
    description: '阿薩姆紅茶搭配香醇鮮奶',
    price: 50
  },
  {
    id: 6,
    name: '檸檬冰茶',
    description: '檸檬與冰茶的清新組合',
    price: 45
  },
  {
    id: 7,
    name: '芒果綠茶',
    description: '芒果與綠茶的獨特風味',
    price: 55
  },
  {
    id: 8,
    name: '抹茶拿鐵',
    description: '抹茶與鮮奶的絕配',
    price: 60
  }
])

const order = ref({})
const cart = ref([])

const addCart = (drink) => {
  const product = cart.value.find((item) => item.name === drink.name)
  if (product) {
    alert(`[${product.name}已加入購物車,請更改需要的數量!]`)
    product.amount += 1
  } else {
    cart.value.push({
      ...drink,
      id: new Date().getTime(),
      amount: 1
    })
  }
}

const updateCart = (item) => {
  cart.value = cart.value.map((cartItem) => {
    if (cartItem.id === item.id) {
      cartItem.sum = parseInt(item.price)
    }
    return cartItem
  })
}

const deleteCart = (id) => {
  cart.value = cart.value.filter((cartItem) => cartItem.id !== id)
}

const itemSum = (item) => {
  return item.price * item.amount
}

const total = computed(() => {
  return cart.value.reduce((pre, next) => {
    return pre + next.price * next.amount
  }, 0)
})

const description = ref('')
const createOrder = () => {
  order.value = {
    id: new Date().getTime(),
    cart: cart.value,
    description: description.value,
    total: total.value
  }
  cart.value = []
  description.value = ''
}
</script>
