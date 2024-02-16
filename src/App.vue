<script setup>
import { ref, watch } from 'vue'
import { debounce } from 'lodash';

const appName = 'Shopping List'
const inputValue = ref('')
const searchSuggestions = ref([])
const currentList = ref([])
const selectedIndex = ref(-1)

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

const selectItem = (index) => {
  const itemName = searchSuggestions.value[index]

  const newItem = {
    name: itemName,
    id: Date.now(),
    checked: false,
    amount: 1
  }
  const itemExists = currentList.value.find(item => item.name === itemName)
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

const increaseAmount = (item) => {
  item.amount++;
}

const decreaseAmount = (item) => {
  if (item.amount > 1) {
    item.amount--;
  }
}

const deleteItem = (item) => {
  const updatedList = currentList.value.filter((product) => item.id !== product.id)
  currentList.value = updatedList
}

const handleKeyDown = (event) => {
  if (event.key === "ArrowDown") {
    selectedIndex.value++
  }
  if (event.key === "ArrowUp") {
    selectedIndex.value--
  }
  if (event.key === 'Enter') {
    selectItem(selectedIndex.value)
  }
}

</script>

<template>
  <main class="flex">
    <div>
        <h1 class="text-4xl mb-6">{{ appName }}</h1>
        <input 
          class="h-8 w-56 px-4 border-2 border-black"
          v-model="inputValue"
          @keydown="handleKeyDown"
        />
        <div 
          class="w-56 border px-6 border-black"
          role="listbox"
          id="Suggested Item"
        > 
          <p
            v-for="(item, index) in searchSuggestions"
            :key="index"
            tabindex="0"
            role="option"
            :id="'Suggested Item - ' + index"
            class="hover:pointer hover:font-bold"
            :class="{'border-2 border-blue-400 px-2': index === selectedIndex}"
            @click="selectItem(index)"
            @keydown.stop.enter.prevent="selectItem(index)"
            :aria-selected="index === selectedIndex"
            @focus="index === selectedIndex"
          >
            {{ item }}
          </p>
        </div>

    </div>
    <div class="flex-row mt-16 ml-16">
        <div
          v-for="(item) in currentList"
          :key="item.id"
          class="flex justify-between mb-2 w-full min-w-[20rem]"
        >
          <div class="flex w-[6rem] ">
            <input 
              class="mr-4"
              type="checkbox" 
              :value="item.name"
              v-model="item.checked"
              @change="updateItemStatus(item.id)"
              @keydown.enter.prevent="updateItemStatus(item.id)"
            />
            <p class="mr-2">{{ item.amount }}</p>
            <button 
              @click="increaseAmount(item)"
              class="w-full"
            >+</button>
            <button 
              @click="decreaseAmount(item)"
              class="w-full"
            >-</button>
          </div>

          <span
            :class="item.checked ? 'line-through text-gray-500' : ''"
          >{{ item.name }}</span>
          <button 
            @click="deleteItem(item)"
          >x
          </button>
        </div>
    </div>
  </main>
</template>