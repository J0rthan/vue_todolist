<script setup>
defineProps({
  todo: { type: Object, required: true }
})

const emit = defineEmits(['toggle', 'remove'])
</script>

<template>
  <div class="item">
    <label class="left">
      <input type="checkbox" :checked="todo.done" @change="emit('toggle', todo.id)" />
      <span class="title" :class="{ done: todo.done }">{{ todo.title }}</span>
    </label>

    <button class="danger" @click="emit('remove', todo.id)">Delete</button>
  </div>
</template>

<style scoped>
.item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  border: 1px solid #eee;
  border-radius: 12px;
  padding: 10px 12px;
}
.left {
  display: flex;
  align-items: center;
  gap: 10px;
}
.title.done {
  text-decoration: line-through;
  color: #888;
}
.danger {
  border: 0;
  background: #ffefef;
  color: #b30000;
  padding: 8px 10px;
  border-radius: 10px;
  cursor: pointer;
}
</style>