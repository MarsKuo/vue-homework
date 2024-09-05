<template>
  <tr>
    <!-- <td><button type="button" class="btn btn-sm" @click="removeFromCart(item.id)">x</button></td> -->
    <td><button type="button" class="btn btn-sm" @click="emit('removeFromCart', item.id)">x</button></td>
    <!-- <td><button type="button" class="btn btn-sm" @click="handleRemoveFromCart(item.id)">x</button></td> -->
    <td>{{ item.name }}</td>
    <td><small>{{ item.description }}</small></td>
    <td>
      <!-- <select class="form-select" v-model="item.quantity" @change="updateCart(item)"> -->
      <select class="form-select" @change="emit('updateCart', item)">
        <!-- <select class="form-select" v-model="item.quantity" @change="handleUpdateCart(item)"> -->
        <option v-for="n in 10" :key="n" :value="n">{{ n }}</option>
      </select>
    </td>
    <td>{{ item.price }}</td>
    <td>{{ item.price * item.quantity }}</td>
  </tr>
</template>

<script setup>
// const props = defineProps({
//   item: {
//     type: Object,
//     required: true,
//   },
// });
// defineProps(['item']);
defineProps(['item']);
const emit = defineEmits(['updateCart', 'removeFromCart']);


// const handleRemoveFromCart = (id) => {
//   emit('removeFromCart', id);
// };

// const handleUpdateCart = (item) => {
//   emit('updateCart', item);
// };


</script>