<script setup>
import TaskTile from './components/TaskTile.vue'
import { ref } from 'vue'
import JsonFile from './assets/tasklist.json'

const taskList = ref(JsonFile)
const newTaskTitle = ref('')
const invalidInput = ref(false)
const pickedFilter = ref('all')

function onAddTaskClicked () {
  if (this.newTaskTitle) {
    let _newTask = {
      'id': this.taskList.length > 0 ? this.taskList.slice(-1)[0].id + 1 : 0,
      'title': this.newTaskTitle,
      'status': 'todo'
    }

    this.taskList.push(_newTask)
    this.invalidInput = false
    this.newTaskTitle = ''
  } else {
    this.invalidInput = true
  }
}

function onDoneTaskClicked (id) {
  let _taskIndex = this.taskList.findIndex(
      (task) => task.id === id
  )
  this.taskList[_taskIndex].status === 'todo'
      ? this.taskList[_taskIndex].status = 'done'
      : this.taskList[_taskIndex].status = 'todo'
}

function onRemoveTaskClicked (id) {
  let _taskIndex = this.taskList.findIndex(
      (task) => task.id === id
  )
  this.taskList.splice(_taskIndex, 1)
}

</script>


<template>
  <h1>
    Seznam úkolů
  </h1>

  <input class="task-input"
         v-model="newTaskTitle"
         placeholder="Zapsat nový úkol"/>
  <div class="task-input__error"
       v-if="invalidInput">
    *Název úkolu je nutné vyplnit
  </div>

  <div class="task-actions">
    <button class="task-input__button"
            @click="onAddTaskClicked()">
      Přidat úkol
    </button>

    <div class="task-actions__filter">
      <div class="task-actions__filter__input">
        <input type="radio" id="all" value="all" v-model="pickedFilter">
        <label for="all">Všechny úkoly</label>
      </div>
      <div class="task-actions__filter__input">
        <input type="radio" id="todo" value="todo" v-model="pickedFilter">
        <label for="todo">Nesplněno</label>
      </div>
      <div class="task-actions__filter__input">
        <input type="radio" id="done" value="done" v-model="pickedFilter">
        <label for="done">Splněno</label>
      </div>
    </div>
  </div>

  <div class="task-list">
    <div v-for="task in taskList" :key="task.id">
      <TaskTile :data="task"
                v-if="pickedFilter === 'all' ? true : task.status === pickedFilter"
                @on-done="(id) => onDoneTaskClicked(id)"
                @on-remove="(id) => onRemoveTaskClicked(id)">

      </TaskTile>
      <div class="task-list__divider"
           v-if="task.id > -1 && task.id < this.taskList.length - 1 && pickedFilter === 'all' ? true : task.status === pickedFilter"/>
    </div>
  </div>

  <div class="task-list__empty"
       v-if="this.taskList.length === 0">
    V tuto chvíli nemáte žádné úkoly
  </div>
</template>

<style>
@import './assets/base.css';

:host {
  display: flex;
  flex-direction: column;
}

h1 {
  padding-bottom: 2rem;
}

.task-actions {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.task-actions__filter {
  display: flex;
  align-items: center;
  column-gap: 1.5rem;
}

.task-actions__filter__input {
  display: flex;
  align-items: center;
  column-gap: .3rem;
}

.task-list__divider {
  height: 1px;
  background: #ececec;
  width: 100%;
}

.task-list {
  display: flex;
  flex-direction: column;
  width: 100%;
  background: white;
  box-shadow: 0 4px 11px #dcdcdd;
  border-radius: .5rem;
}

.task-input__error {
  width: 100%;
  color: red;
}

.task-input {
  width: 100%;
  padding: 1rem;
  border: none;
  border-radius: .5rem;
  font-size: 1.2rem;
  font-weight: 500;
  box-shadow: 0 4px 11px #dcdcdd;
  outline: none;
}

.task-input__button {
  margin: 0.5rem 0 2rem 0;
  box-shadow: 0px 4px 11px #dcdcdd;
}

.task-list__empty {
  width: 100%;
  text-align: center;
  font-size: 1.5rem;
}

#app {
  max-width: 1000px;
  margin: 0 auto;
  padding: 2rem;
}

@media (max-width: 568px) {
  .task-actions {
    flex-direction: column;
  }
  .task-actions__filter {
    width: 100%;
    padding: 1rem 0;
  }
  .task-input__button {
    margin: 0.5rem auto 0 0;
  }
}
</style>
