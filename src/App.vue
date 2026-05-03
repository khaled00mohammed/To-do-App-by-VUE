<template>
  <div class="todo-shell">
    <section class="hero-panel">
      <span class="hero-badge">Vue 3 Premium To-Do</span>
      <h1>{{ title }}</h1>
      <p class="hero-copy">
        Elegant task management built for performance and clarity. Track progress, complete
        items, and keep your workflow focused.
      </p>
    </section>

    <form class="todo-form" @submit.prevent="addTask">
      <input
        v-model.trim="newTaskText"
        type="text"
        placeholder="Type a new task..."
        aria-label="New task"
      />
      <button type="button" class="primary-btn" @click="addTask">Add Task</button>
    </form>

    <div class="status-bar">
      <p class="progress-label">{{ progressLabel }}</p>
      <p class="status-message" v-if="tasks.length === 0">No tasks yet! Add one to get started.</p>
      <p class="status-message" v-else>Keep going — every completed task improves your flow.</p>
    </div>

    <transition-group name="task" tag="ul" class="task-list" v-if="tasks.length > 0">
      <li
        v-for="task in tasks"
        :key="task.id"
        class="task-item"
        :class="{ completed: task.completed }"
      >
        <button class="task-toggle" type="button" @click="toggleTask(task)">
          <span>{{ task.completed ? '✓' : '' }}</span>
        </button>

        <span class="task-text">{{ task.text }}</span>

        <button class="remove-btn" type="button" @click="removeTask(task.id)">✕</button>
      </li>
    </transition-group>

    <div class="empty-state" v-else>
      <p>Start a new list and keep your day on track with clean, modern task cards.</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'

const title = 'Glass Todo Suite'
const newTaskText = ref('')
const tasks = ref([
  { id: 1, text: 'Review UI feedback', completed: false },
  { id: 2, text: 'Prepare sprint planning notes', completed: true },
])

const progressLabel = computed(() => {
  const completed = tasks.value.filter((task) => task.completed).length
  return `${completed}/${tasks.value.length} completed`
})

const addTask = () => {
  const trimmedText = newTaskText.value.trim()
  if (!trimmedText) return

  tasks.value.push({
    id: Date.now(),
    text: trimmedText,
    completed: false,
  })

  newTaskText.value = ''
}

const toggleTask = (task) => {
  task.completed = !task.completed
}

const removeTask = (taskId) => {
  tasks.value = tasks.value.filter((task) => task.id !== taskId)
}

watch(
  () => tasks.value.length,
  () => {
    console.log('Task list updated.')
  },
)
</script>

<style scoped>
:root {
  color-scheme: dark;
}

.todo-shell {
  min-height: 100vh;
  padding: 3rem 1.5rem 4rem;
  display: grid;
  gap: 2rem;
  justify-items: center;
  background: radial-gradient(circle at top, rgba(21, 13, 1, 0.12), transparent 28%),
    radial-gradient(circle at bottom right, rgba(30, 13, 5, 0.18), transparent 26%),
    #432b06;
  color: #e2e8f0;
  font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
}

.hero-panel {
  width: min(1040px, 100%);
  padding: 2.4rem;
  border-radius: 36px;
  background: rgba(104, 57, 4, 0.76);
  border: 1px solid rgba(148, 163, 184, 0.14);
  box-shadow: 0 32px 90px rgba(15, 23, 42, 0.38);
  backdrop-filter: blur(24px);
}

.hero-badge {
  display: inline-flex;
  padding: 0.6rem 1rem;
  border-radius: 999px;
  background: rgba(230, 246, 59, 0.18);
  color: #c9e414;
  font-size: 0.85rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  margin-bottom: 1rem;
}

h1 {
  font-size: clamp(2.4rem, 4vw, 3.5rem);
  margin: 0;
  line-height: 1.03;
  letter-spacing: -0.04em;
}

.hero-copy {
  max-width: 60ch;
  margin: 1rem 0 0;
  color: #cbd5e1;
  font-size: 1.02rem;
  line-height: 1.75;
}

.todo-form {
  width: min(1040px, 100%);
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 1rem;
  margin-top: -2rem;
  padding: 1.5rem;
  background: rgba(15, 23, 42, 0.84);
  border: 1px solid rgba(148, 163, 184, 0.1);
  border-radius: 30px;
  backdrop-filter: blur(20px);
}

.todo-form input {
  width: 100%;
  border: none;
  outline: none;
  border-radius: 20px;
  padding: 1.25rem 1.5rem;
  background: rgba(205, 209, 210, 0.92);
  color: #030000;
  font-size: 1rem;
  box-shadow: inset 0 1px 1px rgba(255, 255, 255, 0.04);
}

.primary-btn {
  border: none;
  cursor: pointer;
  border-radius: 20px;
  padding: 1.25rem 1.8rem;
  min-width: 160px;
  background: linear-gradient(135deg, #e79c2b, #953c19);
  color: white;
  font-weight: 700;
  letter-spacing: 0.02em;
  transition: transform 180ms ease, box-shadow 180ms ease, opacity 180ms ease;
}

.primary-btn:hover {
  transform: translateY(-1px);
  box-shadow: 0 22px 40px rgba(34, 197, 94, 0.18);
}

.status-bar {
  width: min(1040px, 100%);
  display: flex;
  justify-content: space-between;
  gap: 1rem;
  padding: 1rem 1.5rem;
  background: rgba(15, 23, 42, 0.64);
  border: 1px solid rgba(148, 163, 184, 0.12);
  border-radius: 24px;
  backdrop-filter: blur(20px);
}

.progress-label {
  font-weight: 700;
  color: #67f1ac;
}

.status-message {
  color: #94a3b8;
}

.task-list {
  width: min(1040px, 100%);
  list-style: none;
  margin: 0;
  padding: 0;
  display: grid;
  gap: 1rem;
}

.task-item {
  display: grid;
  grid-template-columns: auto 1fr auto;
  align-items: center;
  gap: 1rem;
  padding: 1.35rem 1.5rem;
  border-radius: 24px;
  background: rgba(15, 23, 42, 0.82);
  border: 1px solid rgba(148, 163, 184, 0.1);
  box-shadow: 0 18px 45px rgba(15, 23, 42, 0.2);
  transform-origin: top center;
}

.task-item.completed {
  background: rgba(16, 185, 129, 0.14);
  border-color: rgba(16, 185, 129, 0.3);
}

.task-toggle {
  width: 2.5rem;
  height: 2.5rem;
  border-radius: 50%;
  border: 1px solid rgba(148, 163, 184, 0.4);
  color: #dbeafe;
  background: rgba(15, 23, 42, 0.64);
  display: grid;
  place-items: center;
  font-size: 1rem;
  cursor: pointer;
  transition: background 180ms ease, border-color 180ms ease, color 180ms ease;
}

.task-item.completed .task-toggle {
  background: rgba(16, 185, 129, 0.18);
  border-color: rgba(16, 185, 129, 0.7);
  color: #22c55e;
}

.task-text {
  font-size: 1rem;
  line-height: 1.7;
  color: #e2e8f0;
  transition: color 240ms ease, text-decoration 240ms ease;
}

.task-item.completed .task-text {
  text-decoration: line-through;
  color: #94a3b8;
}

.remove-btn {
  border: none;
  background: transparent;
  color: #94a3b8;
  cursor: pointer;
  font-size: 1.2rem;
  transition: color 160ms ease;
}

.remove-btn:hover {
  color: #f8fafc;
}

.empty-state {
  width: min(1040px, 100%);
  padding: 1.8rem 1.5rem;
  border-radius: 24px;
  background: rgba(15, 23, 42, 0.72);
  border: 1px dashed rgba(148, 163, 184, 0.16);
  color: #cbd5e1;
  text-align: center;
}

.task-enter-from,
.task-leave-to {
  opacity: 0;
  transform: translateY(-16px);
}

.task-enter-active,
.task-leave-active {
  transition: transform 240ms cubic-bezier(0.22, 1, 0.36, 1), opacity 240ms ease;
}

@media (max-width: 720px) {
  .todo-form,
  .status-bar,
  .hero-panel,
  .empty-state,
  .task-list {
    width: 100%;
  }

  .task-item {
    grid-template-columns: 40px 1fr 40px;
  }
}
</style>