<template>
  <div class="common-layout">
    <el-container>
      <el-header> <h1>week02 - 串接 Todo List API</h1></el-header>
      <el-main>
        <h2>{{ isRegister ? '註冊' : isLogin ? 'TodoList' : '登入' }}</h2>
        <div v-if="!isLogin">
          <el-card style="max-width: 400px">
            <el-form style="max-width: 200px" status-icon label-width="auto" class="demo-ruleForm">
              <el-form-item label="帳號" prop="email">
                <el-input v-model="email" type="email" placeholder="輸入信箱" />
              </el-form-item>
              <el-form-item label="密碼" prop="password">
                <el-input
                  v-model="password"
                  type="password"
                  autocomplete="off"
                  placeholder="輸入密碼"
                />
              </el-form-item>
              <div v-if="isRegister">
                <el-form-item label="暱稱" prop="nickname">
                  <el-input v-model="nickname" placeholder="輸入暱稱" />
                </el-form-item>
              </div>
              <el-form-item>
                <el-button type="primary" @click="isRegister ? signUp() : login()">
                  {{ isRegister ? '註冊' : '登入' }}
                </el-button>
                <el-button type="success" @click="register">
                  {{ isRegister ? '返回登入' : '註冊' }}
                </el-button>
              </el-form-item>
            </el-form>
          </el-card>
        </div>

        <div v-else>
          <h2>驗證</h2>
          <el-form style="max-width: 400px" status-icon label-width="auto" class="demo-ruleForm">
            <el-form-item label="驗證Token">
              <el-input v-model="token" type="text" />
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="checkOut()">驗證</el-button>
              <el-button type="primary" @click="signOut">登出</el-button>
            </el-form-item>
          </el-form>
          <!-- todoList -->
          <el-card style="max-width: 400px">
            <h2>TodoList</h2>
            <div>
              <label>請輸入Todo待辦事項</label>
              <input type="text" v-model="inputTodo" />
              <button type="button" @click="addTodo">新增</button>
            </div>
            <ul v-for="todo in todoList" :key="todo.id">
              <li>
                <label v-show="!todo.isEditing">{{ todo.content }}</label>
                <input type="text" v-model="todo.content" v-show="todo.isEditing" />
                <el-form-item>
                  <el-button type="primary" @click="edit(todo)">
                    {{ todo.isEditing ? '確認' : '編輯' }}
                  </el-button>
                  <el-button type="success" @click="removeTodo(todo.id)">刪除</el-button>
                </el-form-item>
              </li>
            </ul>
          </el-card>
        </div>
      </el-main>
    </el-container>
  </div>
</template>

<script lang="ts" setup>
import { reactive, ref } from 'vue'
import type { FormInstance, FormRules } from 'element-plus'
import axios from 'axios'

const api = 'https://todolist-api.hexschool.io'

const email = ref('')
const password = ref('')
const nickname = ref('')
const token = ref('')
const isLogin = ref(false)
const isRegister = ref(false)
const isCheckOut = ref(false)

//註冊
const signUp = async () => {
  console.log(`$(api)/users/sign_up`)
  try {
    const res = await axios.post(`${api}/users/sign_up`, {
      email: email.value,
      password: password.value,
      nickname: nickname.value
    })
    console.log(res)
    alert('註冊完成!')
    register()
  } catch (error) {
    alert('註冊失敗:' + error.message)
  }
}
//登入
const login = async () => {
  try {
    const res = await axios.post(`${api}/users/sign_in`, {
      email: email.value,
      password: password.value
    })
    token.value = res.data.token
    alert('登入成功!')

    //清空
    email.value = ''
    password.value = ''
    isLogin.value = true
  } catch (error) {
    alert('登入失敗:' + error.message)
  }
}

//驗證
const checkOut = async () => {
  try {
    const res = await axios.get(`${api}/users/checkout`, {
      headers: {
        Authorization: token.value
      }
    })
    isCheckOut.value = true
    alert('驗證成功! UID: ' + res.data.uid)
    getTodoList()
  } catch (error) {
    alert('驗證錯誤:' + error.message)
  }
}

//登出
const signOut = async () => {
  try {
    const res = await axios.post(
      `${api}/users/sign_out`,
      {},
      {
        headers: {
          Authorization: token.value
        }
      }
    )

    alert('登出成功!')

    token.value = ''
    isLogin.value = false
  } catch (error) {
    alert('驗證錯誤:' + error.message)
  }
}

//todo
const todoList = ref([])

const getTodoList = async () => {
  const response = await axios.get(`${api}/todos`, {
    headers: {
      Authorization: token.value
    }
  })
  todoList.value = response.data.data.map((todo) => ({ ...todo, isEditing: false }))
}

const inputTodo = ref('')
//新增Todo
const addTodo = async () => {
  try {
    const todo = {
      content: inputTodo.value
    }

    await axios.post(`${api}/todos`, todo, {
      headers: {
        Authorization: token.value
      }
    })

    alert('新增成功')
    inputTodo.value = ''
    getTodoList()
  } catch (error) {
    alert('新增失敗:' + error.message)
  }
}
//刪除Todo
const removeTodo = async (id) => {
  try {
    await axios.delete(`${api}/todos/${id}`, {
      headers: {
        Authorization: token.value
      }
    })
    alert('刪除成功')
    getTodoList()
  } catch (error) {
    alert('刪除失敗:' + error.message)
  }
}
//編輯
const edit = (todo) => {
  if (todo.isEditing) {
    updateTodo(todo.id)
  } else {
    todo.isEditing = true
  }
}
//更新
const updateTodo = async (id) => {
  try {
    const todo = todoList.value.find((todo) => todo.id === id)

    await axios.put(`${api}/todos/${id}`, todo, {
      headers: {
        Authorization: token.value
      }
    })

    getTodoList()
    alert('編輯成功')
  } catch (error) {
    alert('編輯失敗:' + error.message)
  }
}

//註冊
const register = () => {
  isRegister.value = !isRegister.value
  email.value = ''
  password.value = ''
  nickname.value = ''
}
</script>
<style>
h2 {
  text-align: left;
  color: rgb(61, 42, 145);
  margin: 10px;
}
</style>
