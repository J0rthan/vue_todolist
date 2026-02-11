<script setup>
import { computed, onMounted, ref, watch } from 'vue'
import TodoInput from './components/TodoInput.vue'
import TodoFilter from './components/TodoFilter.vue'
import TodoList from './components/TodoList.vue'

/**
 * 数据结构：
 * todo = { id: string, title: string, done: boolean, createdAt: number }
 */
const todos = ref([])
const filter = ref('all') // all | active | done

// 统计（计算属性）
const totalCount = computed(() => todos.value.length)
const doneCount = computed(() => todos.value.filter(t => t.done).length)
const activeCount = computed(() => totalCount.value - doneCount.value)

// 过滤后的列表（计算属性）
const filteredTodos = computed(() => {
  if (filter.value === 'active') return todos.value.filter(t => !t.done)
  if (filter.value === 'done') return todos.value.filter(t => t.done)
  return todos.value
})

// 业务操作
function addTodo(title) { // 增加todo
  const trimmed = title.trim()
  if (!trimmed) return // function unshift() description: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift
  todos.value.unshift({
    id: crypto.randomUUID(),
    title: trimmed,
    done: false,
    createdAt: Date.now()
  })
}

function toggleTodo(id) { // 改变todo属性
  const t = todos.value.find(x => x.id === id)
  if (t) t.done = !t.done
}

function removeTodo(id) { // 删除done状态
  todos.value = todos.value.filter(t => t.id !== id)
}

function setFilter(next) { // 切换filter状态
  filter.value = next
}

// 本地持久化：加载 + 监听保存
const STORAGE_KEY = 'todolite.todos'

onMounted(() => {
  try {
    const raw = localStorage.getItem(STORAGE_KEY)
    if (raw) todos.value = JSON.parse(raw)
  } catch (e) {
    console.warn('Failed to load todos:', e)
  }
})

watch(
  todos,
  (val) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(val))
  },
  { deep: true }
)
</script>

<template>
  <div class="page">
    <header class="header">
      <h1>TodoList</h1>
      <p class="sub">
        Total: {{ totalCount }} | Active: {{ activeCount }} | Done: {{ doneCount }}
      </p>
    </header>

    <main class="card">
      <TodoInput @add="addTodo"></TodoInput>

      <TodoFilter :model-value="filter" @update:model-value="setFilter" />

      <TodoList
        :todos="filteredTodos"
        @toggle="toggleTodo"
        @remove="removeTodo"
      />
    </main>
  </div>
</template>

<style scoped>
.page {
  min-height: 100vh;
  display: grid;
  place-items: center;
  padding: 32px;
  background: #f6f7fb;
  color: #111;
  font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial;
}
.card {
  width: min(720px, 92vw);
  background: #fff;
  border-radius: 14px;
  padding: 20px;
  box-shadow: 0 12px 30px rgba(0,0,0,.08);
}
.header {
  width: min(720px, 92vw);
  margin-bottom: 14px;
}
.header h1 {
  margin: 0 0 6px;
  font-size: 28px;
}
.sub {
  margin: 0;
  color: #555;
  font-size: 14px;
}
</style>