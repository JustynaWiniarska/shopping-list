<script setup>
import { ref, watch } from 'vue'

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
  setTimeout(() => {
    if (inputValue.value.length > 2) {
      fetchItems()
    }
  }, 500)
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
  <main>
    <div>
        <h1>{{ appName }}</h1>
        <input 
          class="search-input"
          v-model="inputValue"
        />
        <div class="suggested-items"> 
          <p
            v-for="(item, index) in searchSuggestions"
            :key="index"
            class="suggested-item"
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
          class="shopping-list-item"
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
main {
  display: flex;
}
.search-input {
  height: 35px;
  width: 200px;
}
.suggested-items {
  border: 1px solid black;
  width: 200px;
  padding-left: 20px;
}
.suggested-item:hover {
  font-weight: bold;
  cursor: pointer;
}
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
  font-style: italic;
  text-decoration: line-through;
}
.delete-btn {
  width: 20px;
}


</style>
