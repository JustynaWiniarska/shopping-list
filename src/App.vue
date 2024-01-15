<script setup>
import { ref, watch } from 'vue'
import { debounce } from 'lodash';

const appName = 'Shopping List'
const inputValue = ref('')
const searchSuggestions = ref([])
const currentList = ref([])

const fetchItems = async () => {
  const response = await fetch(`https://api.frontendeval.com/fake/food/${inputValue.value}`)
  const data = await response.json()
  searchSuggestions.value = data
}

watch(inputValue, () => {
  if (inputValue.value.length > 1) {
    const debouncedFetchItems = debounce(() => {
      fetchItems()
    }, 500)
    debouncedFetchItems()
  }
})

const selectItem = (name) => {
  const newItem = {
    name,
    id: Date.now(),
    checked: false
  }
  const itemExists = currentList.value.find(item => item.name === name)
  if (!itemExists) {
    currentList.value.push(newItem)
  } 
}

const updateItemStatus = (id) => {
  currentList.value.forEach((item) => {
    if (item.id === id) {
      item.checked = !item.checked
    }
  })
}

const deleteItem = (item) => {
  const updatedList = currentList.value.filter((product) => item.id !== product.id)
  currentList.value = updatedList
}
</script>

<template>
  <main class="flex">
    <div>
        <h1 class="text-4xl mb-6">{{ appName }}</h1>
        <input 
          class="h-8 w-56 px-4 border-2 border-black"
          v-model="inputValue"
        />
        <div class="w-56 border px-6 border-black"> 
          <p
            v-for="(item, index) in searchSuggestions"
            :key="index"
            class="hover:pointer hover:font-bold"
            @click="selectItem(item)"
          >
            {{ item }}
          </p>
        </div>

    </div>
    <div
      class="shopping-list"
    >
        <div
          v-for="(item) in currentList"
          :key="item.id"
          class="shopping-list-item w-full"
        >
          <input 
            type="checkbox" 
            :value="item.name"
            @change="updateItemStatus(item.id)"
          />
          <span
            :class="item.checked ? 'checked-off' : ''"
          >{{ item.name }}</span>
          <button 
            class="delete-btn"
            @click="deleteItem(item)"
          >x
          </button>
        </div>
    </div>
  </main>
</template>

<style scoped>
button {
  width: 100%
}
.shopping-list {
  margin: 3rem 0 0 3rem;
  flex-direction: row;
}
.shopping-list-item {
  display: flex;
  justify-content: space-between;
  min-width: 250px;
  margin-bottom: 15px;
}
.checked-off {
  text-decoration: line-through;
  color: grey;
}
.delete-btn {
  width: 20px;
}


</style>
