<template>
  <div id="root">
    <div class="container mt-5">
      <div class="row">
        <div class="col-md-4">
          <div class="list-group">

            <a href="#" class="list-group-item list-group-item-action" v-for="drink in drinks" :key="drink.id" @click="addToCart(drink)">
              <div class="d-flex w-100 justify-content-between">
                <h5 class="mb-1"> {{ drink.name }}</h5>
                <small>{{ `$${drink.price}` }}</small>
              </div>
              <p class="mb-1">{{ drink.description }}</p>
            </a>


          </div>
        </div>
        <div class="col-md-8">
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
              <!-- <tr v-for="item in cart" :key="item.id">
                <td><button type="button" class="btn btn-sm" @click="removeFromCart(item.id)">x</button></td>
                <td>{{ item.name }}</td>
                <td><small>{{ item.description }}</small></td>
                <td>
                  <select class="form-select" v-model="item.quantity" @change="updateCart(item)">
                    <option v-for="n in 10" :key="n" :value="n">{{ n }}</option>
                  </select>
                </td>
                <td>{{ item.price }}</td>
                <td>{{ item.price * item.quantity }}</td>
              </tr> -->

              <CartItem v-for="item in cart" :key="item.id" :item="item" @removeFromCart="removeFromCart" @updateCart="updateCart" />
            </tbody>
          </table>
          <div v-if="cart.length === 0" class="alert alert-primary text-center" role="alert">
            請選擇商品
          </div>
          <div class="text-end mb-3">
            <h5>總計: <span>$ {{ sum }}</span></h5>
          </div>
          <textarea class="form-control mb-3" rows="3" placeholder="備註"></textarea>
          <div class="text-end">
            <button class="btn btn-primary" @click="createOrder">送出</button>
          </div>
        </div>
      </div>
      <hr />
      <div class="row justify-content-center">
        <div class="col-8">
          <div v-if="!order.id" class="alert alert-secondary text-center" role="alert">
            尚未建立訂單
          </div>
          <OrderItem class="card" :order="order" v-else />

        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';
import CartItem from '@/views/Component/CartItem.vue';
import OrderItem from '@/views/Component/OrderItem.vue';
const data = [
  {
    "id": 1,
    "name": "珍珠奶茶",
    "description": "香濃奶茶搭配QQ珍珠",
    "price": 50
  },
  {
    "id": 2,
    "name": "冬瓜檸檬",
    "description": "清新冬瓜配上新鮮檸檬",
    "price": 45
  },
  {
    "id": 3,
    "name": "翡翠檸檬",
    "description": "綠茶與檸檬的完美結合",
    "price": 55
  },
  {
    "id": 4,
    "name": "四季春茶",
    "description": "香醇四季春茶，回甘無比",
    "price": 45
  },
  {
    "id": 5,
    "name": "阿薩姆奶茶",
    "description": "阿薩姆紅茶搭配香醇鮮奶",
    "price": 50
  },
  {
    "id": 6,
    "name": "檸檬冰茶",
    "description": "檸檬與冰茶的清新組合",
    "price": 45
  },
  {
    "id": 7,
    "name": "芒果綠茶",
    "description": "芒果與綠茶的獨特風味",
    "price": 55
  },
  {
    "id": 8,
    "name": "抹茶拿鐵",
    "description": "抹茶與鮮奶的絕配",
    "price": 60
  }
];
const drinks = ref(data);
const cart = ref([]);
const order = ref({});
const description = ref('');
const addToCart = (drink) => {
  console.log(drink);
  cart.value.push({
    ...drink, id: new Date().getTime(),
    quantity: 1
  });
  console.log(cart.value);
}

const sum = computed(() => {
  return cart.value.reduce((pre, next) => {
    return pre + next.price * next.quantity;
  }, 0);
});
const updateCart = (item) => {
  console.log(item);
  cart.value = cart.value.map((cartItem) => {
    if (cartItem.id === item.id) {
      cartItem.quantity = parseInt(item.quantity);
    } else {
      return cartItem;
    }
  })
}
const removeFromCart = (id) => {
  console.log(id);
  console.log(cart.value);
  cart.value = cart.value.filter((item) => item.id !== id);
}

const createOrder = () => {
  order.value = {
    id: new Date().getTime(),
    cart: cart.value,
    description: description.value,
    sum: sum.value,
    note: ''
  }
  cart.value = [];
  description.value = '';
}
</script>

<style scoped></style>