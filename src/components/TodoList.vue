<script setup>
import TodoItem from './TodoItem.vue'

defineProps({
  todos: { type: Array, default: () => [] }
})

const emit = defineEmits(['toggle', 'remove'])

function onToggle(id) {
  emit('toggle', id)
}
function onRemove(id) {
  emit('remove', id)
}
</script>

<template>
  <div class="list">
    <p v-if="todos.length == 0" class="empty">No todos.</p>

    <TodoItem
        v-for="t in todos"
        :key="t.id"
        :todo="t"
        @toggle="onToggle"
        @remove="onRemove"
    ></TodoItem>
  </div>
</template>

<style scoped>
.list {
  margin-top: 10px;
  display: grid;
  gap: 8px;
}
.empty {
  margin: 14px 0 0;
  color: #777;
  font-size: 14px;
}
</style>