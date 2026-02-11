<script setup>
import { ref } from 'vue'

const title = ref('')

const emit = defineEmits(['add'])

function submit() {
    emit('add', title.value)
    title.value = ''
}

</script>

<template>
  <form class="row" @submit.prevent="submit">
    <input
      class="input"
      placeholder="Add a todo..."
      v-model="title"
      maxlength="60"
    />
    <button class="btn" type="submit">Add</button>
  </form>
</template>

<style scoped>
.row {
  display: flex;
  gap: 10px;
}
.input {
  flex: 1;
  padding: 10px 12px;
  border: 1px solid #ddd;
  border-radius: 10px;
  outline: none;
}
.input:focus {
  border-color: #999;
}
.btn {
  padding: 10px 14px;
  border: 0;
  border-radius: 10px;
  cursor: pointer;
  background: #111;
  color: #fff;
}
</style>