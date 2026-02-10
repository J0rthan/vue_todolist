<script setup>
const props = defineProps({
  modelValue: { type: String, default: 'all' }
})
const emit = defineEmits(['update:modelValue'])

function set(value) {
  emit('update:modelValue', value)
}
</script>

<template>
  <div class="bar">
    <button class="chip" :class="{ active: modelValue === 'all' }" @click="set('all')">
      All
    </button>
    <button class="chip" :class="{ active: modelValue === 'active' }" @click="set('active')">
      Active
    </button>
    <button class="chip" :class="{ active: modelValue === 'done' }" @click="set('done')">
      Done
    </button>
  </div>
</template>

<style scoped>
.bar {
  display: flex;
  gap: 8px;
  margin: 14px 0;
}
.chip {
  padding: 8px 10px;
  border-radius: 999px;
  border: 1px solid #ddd;
  background: #fff;
  cursor: pointer;
  font-size: 13px;
}
.chip.active {
  border-color: #111;
  background: #111;
  color: #fff;
}
</style>