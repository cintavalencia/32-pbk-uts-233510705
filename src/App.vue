<script setup>
import { ref } from 'vue';

const items = ref([]);
const newItem = ref('');

const addItem = () => {
  if (newItem.value !== '') {
    items.value.push({
      id: items.value.length + 1,
      text: newItem.value,
      completed: false
    });
    newItem.value = '';
    console.log(items.value);
  }
};

const toggleItem = (item) => {
  item.completed = !item.completed;
  console.log(item.completed);
}

const removeItem = (item) => {
  items.value = items.value.filter(i => i !== item);
}
</script>

<template>
  <h1 class="text-red-500">todo list</h1>
  <input type="text" v-model="newItem" @keyup.enter="addItem" />
  <button @click="addItem">Tambah</button>

  <ul>
    <li v-for="item in items" :key="item.id">
      <input type="checkbox" :checked="item.completed" @change="toggleItem(item)" />
      {{ item.text }}
      <button @click="removeItem(item)">Hapus</button>
    </li>
  </ul>
</template>

<style scoped></style>
