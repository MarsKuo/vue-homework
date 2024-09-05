<template>
  <div class="p-1">
    <section>
      <h2>註冊</h2>
      <input type="email" placeholder="Email" v-model="emailSignup">
      <input type="password" placeholder="Password" v-model="passwordSignup">
      <input type="Nick Name" placeholder="Nick Name" v-model="nickNameSignup">
      <button @click="signup">註冊</button>
      <p>{{ messageSignup }}</p>
    </section>
    <hr>
    <section>
      <h2>登入</h2>
      <input type="email" placeholder="Email" v-model="emailSignin">
      <input type="password" placeholder="Password" v-model="passwordSignin">

      <button @click="signin">登入</button>
      <p>Token: {{ token }}</p>
      <p>{{ messageSignin }}</p>
    </section>
    <hr>
    <section>
      <h2>驗證</h2>
      <input type="text" placeholder="Token" v-model="tokenCheckout">
      <button @click="checkout">Checkout</button>
      <p>{{ messageCheckout }}</p>
    </section>
    <hr>
    <section>
      <h2>登出</h2>
      <input type="text" placeholder="Token">
      <button @click="signout">登出</button>
      <p>{{ tokenSignout }}</p>
    </section>
    <hr>
    <section>
      <h2>Todo list:</h2>
      <div v-if="token">
        <input type="text" placeholder="Todo" v-model="newTodo">
        <button @click="addTodo">Add</button>

        <ul>
          <li v-for="(todo, index) in todos" :key="index">
            {{ todo.content }} {{ todo.status ? '完成' : '未完成' }} |
            {{ todoEdit[todo.id] }}
            <input type="text" placeholder="更新值" @change="updateTodoEdit($event, todo.id)" />

            <button @click="deleteTodo(todo.id)">Delete</button>
            <button @click="updateTodo(todo.id)">Update</button>
            <button @click="toggleStatus(todo.id)">Toggle Status</button>
          </li>
        </ul>
      </div>
    </section>



  </div>

</template>

<script setup>
import axios from 'axios';
import { onMounted, ref } from 'vue';


const apiUrl = "https://todolist-api.hexschool.io";


//註冊
const emailSignup = ref("");
const passwordSignup = ref("");
const nickNameSignup = ref("");
const messageSignup = ref("");

const signup = async () => {
  try {
    const response = await axios.post(`${apiUrl}/users/sign_up`, {
      email: emailSignup.value,
      password: passwordSignup.value,
      nickname: nickNameSignup.value,

    })
    console.log(response);
    messageSignup.value = "註冊成功. UID: " + response.data.uid;
  } catch (error) {
    messageSignup.value = "註冊失敗" + error.response.data.message;
  }
}

//登入
const emailSignin = ref("");
const passwordSignin = ref("");
const token = ref("");
const messageSignin = ref("");
const signin = async () => {
  try {
    const response = await axios.post(`${apiUrl}/users/sign_in`, {
      email: emailSignin.value,
      password: passwordSignin.value,
    })
    // console.log(response);
    token.value = response.data.token;
  } catch (error) {
    messageSignin.value = "登入失敗" + error.message;
  }
}

//驗證
const tokenCheckout = ref("");
const messageCheckout = ref("");
const checkout = async () => {
  const tomorrow = new Date();
  tomorrow.setDate(tomorrow.getDate() + 1);
  document.cookie = `hexschoolTodo=${tokenCheckout.value}; expires=${tomorrow.toUTCString()}`;
  try {
    const response = await axios.get(`${apiUrl}/users/checkout`, {
      headers: {
        Authorization: `${tokenCheckout.value}`
      }
    })
    console.log(response.data);
    messageCheckout.value = '驗證成功 ';
  } catch (error) {
    messageCheckout.value = "驗證錯誤" + error.message;
  }
}



//登出
const tokenSignout = ref("");
const signout = async () => {
  console.log(tokenCheckout.value);
  try {
    const response = await axios.post(`${apiUrl}/users/sign_out`, {}, {
      headers: {
        Authorization: `${tokenCheckout.value}`
      }
    })
    console.log(response);
    tokenSignout.value = "登出成功";
  } catch (error) {
    tokenSignout.value = "登出失敗" + error.message;
  }
}


//todo list
const todos = ref([]);
const newTodo = ref("");
const todoEdit = ref([]);

const getTodoList = async () => {
  try {
    const response = await axios.get(`${apiUrl}/todos`, {
      headers: {
        Authorization: `${token.value}`
      }
    })
    todos.value = response.data.data;
    console.log(response);
  } catch (error) {
    console.log(error);
  }
}

const addTodo = async () => {
  if (!newTodo.value) return;

  const todo = {
    content: newTodo.value,
  };
  console.log(todo.value, token.value);
  try {
    const response = await axios.post(`${apiUrl}/todos`, todo, {
      headers: {
        Authorization: `${token.value}`
      }
    })
    console.log(response);
    getTodoList();
  } catch (error) {
    console.log(error);
  }
}

const deleteTodo = async (id) => {
  await axios.delete(`${apiUrl}/todos/${id}`, {
    headers: {
      Authorization: token.value,
    },
  });
  getTodoList();
};

const updateTodo = async (id) => {
  const todo = todos.value.find((todo) => todo.id === id);
  todo.content = todoEdit.value[id];
  await axios.put(`${apiUrl}/todos/${id}`, todo, {
    headers: {
      Authorization: token.value,
    },
  });
  getTodoList();
  todoEdit.value = {
    ...todoEdit.value,
    [id]: '',
  };
};

const toggleStatus = async (id) => {
  await axios.patch(
    `${apiUrl}/todos/${id}/toggle`,
    {},
    {
      headers: {
        Authorization: token.value,
      },
    }
  );
  getTodoList();
};

const updateTodoEdit = (event, id) => {
  todoEdit.value = {
    ...todoEdit.value,
    [id]: event.target.value,
  };
};

//從cookie中取得token
const TodoToken = document.cookie
  .split('; ')
  .find((row) => row.startsWith('hexschoolTodo='))
  ?.split('=')[1];

onMounted(() => {
  if (TodoToken) {
    token.value = TodoToken;
  }
});

</script>
<style scoped></style>