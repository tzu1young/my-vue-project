<script setup>
import { ref, watchEffect } from 'vue'
const BASE_URL = import.meta.env.VITE_APIURL
const API_URL = `${BASE_URL}/productlist/GetAllCategory`
const categories = ref([])
const categoryid = ref(0)
const emit = defineEmits(["categoryClick"])
const clickHandler = id => {
  categoryid.value = id
  emit("categoryClick", id)
}
watchEffect(async () => {
  //用 fetch() 呼叫 API
  const response = await fetch(API_URL)
  const datas = await response.json();
  categories.value = datas;
})
</script>
<template>
  <div>
    <ul v-if="categories && categories.length > 0" class="list-group list-group-horizontal">
      <li :class="{ 'current': category.categoryid === categoryid, 'list-group-item': true }"
        v-for="category in categories" :key="category.categoryid" @click="clickHandler(category.categoryid)">
        <button type="button" class="btn">{{ category.categoryname }}</button>
      </li>
    </ul>
  </div>
</template>
<style lang="css" scoped>
.current {
  background-color: lightblue;
}
</style>
