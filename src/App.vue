<script setup lang="ts">
import {IItem, STORE} from "./store";

import ItemList from "./components/ItemList.vue";
import {ref} from "vue";

const userItems = ref(STORE.userItems);
const toChooseItems = ref(STORE.toChooseItems);

const userSelectedItemsStack = ref<IItem[]>([]);
const selectedItem = ref<IItem | null>(null);

const toggleUserItem = (id: number) => {
  const item = userItems.value.find(item => item.id === id);
  if (!item) return;
  const existingIndex = userSelectedItemsStack.value.findIndex(item => item.id === id);
  if (existingIndex !== -1) {
    userSelectedItemsStack.value.splice(existingIndex, 1);
  } else {
    userSelectedItemsStack.value.push(item);
  }
}

const toggleToChooseItem = (id: number) => {
  const item = toChooseItems.value.find(item => item.id === id);
  if (!item) return;
  selectedItem.value = selectedItem.value === item ? null : item;
}
</script>

<template>
  <main class="container">
    <ItemList @toggle-item="toggleUserItem" :items="userSelectedItemsStack"/>
    <ItemList @toggle-item="toggleToChooseItem" :item="selectedItem"/>
    <ItemList @toggle-item="toggleUserItem" :items="userItems"/>
    <ItemList @toggle-item="toggleToChooseItem" :items="toChooseItems"/>
  </main>
</template>

<style lang="scss" scoped>
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto auto;
  grid-gap: 2rem;
}
</style>
