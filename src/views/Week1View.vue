<script setup>
var data = [
  {
    "name": "珍珠奶茶",
    "description": "香濃奶茶搭配QQ珍珠",
    "price": 50,
    "quantity": 20
  },
  {
    "name": "冬瓜檸檬",
    "description": "清新冬瓜配上新鮮檸檬",
    "price": 45,
    "quantity": 18
  },
  {
    "name": "翡翠檸檬",
    "description": "綠茶與檸檬的完美結合",
    "price": 55,
    "quantity": 34
  },
  {
    "name": "四季春茶",
    "description": "香醇四季春茶，回甘無比",
    "price": 45,
    "quantity": 10
  },
  {
    "name": "阿薩姆奶茶",
    "description": "阿薩姆紅茶搭配香醇鮮奶",
    "price": 50,
    "quantity": 25
  },
  {
    "name": "檸檬冰茶",
    "description": "檸檬與冰茶的清新組合",
    "price": 45,
    "quantity": 20
  },
  {
    "name": "芒果綠茶",
    "description": "芒果與綠茶的獨特風味",
    "price": 55,
    "quantity": 18
  },
  {
    "name": "抹茶拿鐵",
    "description": "抹茶與鮮奶的絕配",
    "price": 60,
    "quantity": 20
  }
];

import { ref } from 'vue';

const items = ref(data);
const editingIndex = ref(-1);
const editValue = ref('');

function startEdit(index) {
  editingIndex.value = index;
  editValue.value = items.value[index].name;
}

function finishEditing() {
  if (editingIndex.value !== -1) {
    items.value[editingIndex.value].name = editValue.value;
  }
  editingIndex.value = -1;
}
</script>

<template>
  <div class="p-3">
    <table>
      <thead>
        <tr>
          <th scope="col">品項</th>
          <th scope="col">描述</th>
          <th scope="col">價格</th>
          <th scope="col">庫存</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in items" :key="index">
          <td @click.stop>
            <template v-if="editingIndex === index">
              <input v-model="editValue" @blur="finishEditing" @keyup.enter="finishEditing">
            </template>
            <template v-else>
              <span @dblclick="startEdit(index)">{{ item.name }}</span>
            </template>
          </td>
          <td>{{ item.description }}</td>
          <td>{{ item.price }}</td>
          <td>
            <button @click="item.quantity--">-</button>
            {{ item.quantity }}
            <button @click="item.quantity++">+</button>
          </td>
        </tr>


      </tbody>
    </table>
  </div>

</template>

<style></style>
