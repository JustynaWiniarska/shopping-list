<script setup>
import { ref, watch } from 'vue'

  const appName = 'Shopping List'
  const inputValue = ref('')
  const suggestedItems = ref([])
  
  const fetchItems = async () => {
    const response = await fetch(`https://api.frontendeval.com/fake/food/${inputValue.value}`)
    const data = await response.json()
    suggestedItems.value = data
    
    console.log(data, suggestedItems.value)
  }

  watch(inputValue, () => {
    setTimeout(() => {
      fetchItems()
    }, 500)
    
  })

</script>

<template>
  <main>
    <div>
      <h1>{{ appName }}</h1>
      <input 
        class="search-input"
        v-model="inputValue"
      />
    </div>
    <div>
      <p>{{ inputValue }}</p>
    </div>
  </main>
</template>

<style scoped>
.search-input {
  height: 35px;
  width: 200px;
}
</style>
