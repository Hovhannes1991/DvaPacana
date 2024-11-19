<template>
  <div class="item-list">
    <div v-if="isEmpty" class="is-empty">
      <span>There is no selected item(s)</span>
    </div>

    <ul v-else-if="Array.isArray(items)">
      <li
          @click="emit('toggleItem', item.id)"
          v-for="item in items"
          :key="item.id"
      >
        {{ item.name }}
      </li>
    </ul>

    <ul v-else-if="item" class="single-item">
      <li @click="emit('toggleItem', item.id)">{{ item.name }}</li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import type {IItem} from "../store";
import {computed} from "vue";

interface IProps {
  items?: IItem[];
  item?: IItem | null;
}

const props = defineProps<IProps>();
const emit = defineEmits(["toggleItem"]);

const isEmpty = computed(() => {
  if(Array.isArray(props.items)) return !props.items.length;
  return !props.item;
})
</script>

<style lang="scss" scoped>
.item-list {
  min-height: 20rem;
  padding: 1rem;
  border: 2px solid var(--black);

  ul {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    list-style-type: none;

    li {
      display: flex;
      align-items: center;
      justify-content: center;
      width: 10rem;
      height: 10rem;
      font-size: 1.8rem;
      cursor: pointer;
      padding: 0.5rem;
      border: 2px solid var(--primary);
      border-radius: 0.5rem;
    }

    &.single-item {
      width: 100%;
      height: 100%;

      li {
        width: 100%;
        height: 100%;
      }
    }
  }

  .is-empty {
    font-size: 2rem;
    font-style: italic;
  }
}
</style>
