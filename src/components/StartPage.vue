<script setup lang="ts">
import { ref } from 'vue'
import { v4 as uuidv4 } from 'uuid'
import Button from 'primevue/button'
import Checkbox from 'primevue/checkbox'
import InputText from 'primevue/inputtext'
import FloatLabel from 'primevue/floatlabel'

defineProps<{
  msg: string
}>()

interface Task {
  todo: string
  id: string
  done: boolean
}

const newTask = ref('')
const tasks = ref([] as Task[])
const tasksDone = ref([] as Task[])

function addTask() {
  if (newTask.value) {
    tasks.value.push({ todo: newTask.value, id: uuidv4(), done: false })
  }
  newTask.value = ''
}

function toggleChecked(task: Task, index: number) {
  if (!task.done) {
    tasksDone.value.push(task)
    tasks.value.splice(index, 1)
  } else if (task.done) {
    tasksDone.value.splice(index, 1)
    tasks.value.push(task)
  }
  task.done = !task.done
}

function removeFromTasksDone(index: number) {
  tasksDone.value.splice(index, 1)
}

function removeFromTasks(index: number) {
  tasks.value.splice(index, 1)
}
</script>

<template>
  <div class="todo-wrapper">
    <h1>{{ msg }}</h1>
    <FloatLabel variant="on">
      <InputText id="on_label" v-model="newTask" autocomplete="off" />
      <label for="on_label">Add todo here</label>
    </FloatLabel>

    <Button @click="addTask" label="Success" variant="outlined">Add</Button>
    <h4 v-if="tasks.length">Things to do</h4>
    <ul>
      <li v-for="(task, index) in tasks" :key="task.id" class="todoItem">
        <span>
          <Checkbox v-model="task.done" binary @click="toggleChecked(task, index)" />
          {{ task.todo }}
        </span>
        <Button @click="removeFromTasks(index)">x</Button>
      </li>
    </ul>
    <h4 v-if="tasksDone.length">Already done</h4>
    <ul>
      <li v-for="(task, index) in tasksDone" :key="task.id" class="todoItem done">
        <span>
          <Checkbox v-model="task.done" binary @click="toggleChecked(task, index)" />
          {{ task.todo }}
        </span>
        <Button @click="removeFromTasksDone(index)" class="removeTask">x</Button>
      </li>
    </ul>
  </div>
</template>

<style>
.todo-wrapper {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  border: 2px solid black;
  width: 60%;
  margin: auto;
  padding: 1.5rem;
  background-color: rgb(241, 248, 253);
  box-shadow: 5px 5px 20px black;
}

.todoItem {
  width: 100%;
  min-width: 600px;
  background-color: var(--p-pink-100);
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 1rem;
  padding: 0 0 0 1rem;
}

.done {
  width: 100%;
  background-color: var(--p-pink-700);
}
</style>
