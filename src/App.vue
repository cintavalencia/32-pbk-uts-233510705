<script setup>
import { ref, computed } from 'vue';

const items = ref([]);
const newItem = ref('');
const filter = ref('all');

const addItem = () => {
  if (newItem.value !== '') {
    items.value.push({
      id: items.value.length + 1,
      text: newItem.value,
      completed: false
    });
    newItem.value = '';
  }
};

const toggleItem = (item) => {
  item.completed = !item.completed;
}

const removeItem = (item) => {
  items.value = items.value.filter(i => i !== item);
}

const filteredItems = computed(() => {
  if (filter.value === 'all') {
    return items.value;
  } else if (filter.value === 'completed') {
    return items.value.filter(item => item.completed);
  } else {
    return items.value.filter(item => !item.completed);
  }
})

</script>

<template>
  <div class="min-h-screen bg-[#EBD9FF] flex flex-col items-center px-4 relative overflow-hidden">
    <!-- Container -->
    <div class="w-full max-w-2xl bg-white rounded-2xl shadow-xl p-8 z-10">
      <!-- Header -->
      <div class="flex justify-between items-center mb-6">
        <h1 class="text-3xl font-extrabold text-[#C77DFF]">Your Daily Tasks</h1>
        <select 
          v-model="filter"
          class="border border-[#C77DFF] rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-[#C77DFF] bg-white text-sm"
        >
          <option value="all">All</option>
          <option value="completed">Completed</option>
          <option value="active">Active</option>
        </select>
      </div>

      <!-- Input -->
      <div class="flex mb-4">
        <input 
          type="text" 
          v-model="newItem" 
          @keyup.enter="addItem" 
          placeholder="What do you need to do today?" 
          class="flex-1 border border-[#C77DFF] rounded-l-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-[#C77DFF]"
        />
        <button 
          @click="addItem" 
          class="bg-[#FFC1CC] text-white px-5 py-2 rounded-r-lg hover:bg-[#FFA3B1] transition-colors duration-300"
        >
          Add
        </button>
      </div>

      <!-- Task List -->
      <div class="bg-[#F7F7F7] rounded-xl p-4 h-[400px] overflow-y-auto">
        <ul class="space-y-3">
          <li 
            v-for="item in filteredItems" 
            :key="item.id"
            class="bg-white p-3 rounded-lg shadow border border-[#FADADD] flex items-center"
          >
            <input 
              type="checkbox" 
              :checked="item.completed" 
              @change="toggleItem(item)"
              class="mr-3 h-5 w-5 text-[#C77DFF] rounded focus:ring-[#C77DFF]"
            />
            <span 
              :class="{'line-through text-pink-300': item.completed, 'text-gray-800': !item.completed}"
              class="flex-1 transition-all duration-300"
            >
              {{ item.text }}
            </span>
            <button 
              @click="removeItem(item)"
              class="text-[#C77DFF] hover:text-[#a25cc9] ml-2 p-1"
            >
              <!-- Trash icon -->
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
              </svg>
            </button>
          </li>
        </ul>

        <!-- Empty State -->
        <div v-if="filteredItems.length === 0" class="text-center py-8 text-[#C77DFF]">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-12 w-12 mx-auto mb-3 animate-bounce" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2" />
          </svg>
          <p class="text-sm">You're all caught up! 💖</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
li {
  animation: fadeIn 0.5s ease-out;
}
</style>

