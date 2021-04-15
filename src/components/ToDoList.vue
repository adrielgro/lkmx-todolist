<template>
  <div class="todolist">
    <h2>To Do List</h2>
    <div class="todolist-form">
      <input v-model="newTask" @keydown.enter="addTask" placeholder="Escribe una tarea" type="text" />
      <button @click="addTask" :disabled="isEmptyInput">Agregar</button>
    </div>
    <div class="todolist-list">
      <ul>
        <li class="todolist-list-elements" v-for="(task, idx) in tasks" :key="idx">
          <span v-if="!task.editing">{{ `• ${task.text}` }} </span>
          <input v-else type="text" v-model="task.text" />

          <div class="todolist-list-actions">
            <button @click="editTask(task)" v-show="!task.editing">
              <IconEdit />
            </button>
            <button @click="removeTask(idx)" v-show="!task.editing">
              <IconRemove />
            </button>
            <button @click="saveTask(task)" v-show="task.editing" :disabled="!task.text">
              <IconSave />
            </button>
          </div>
        </li>
      </ul>
      <span v-show="isEmptyTasks">No has agregado tareas</span>
    </div>
  </div>
</template>

<script>
// Elements SVG
import IconEdit from '../assets/icons/icon-pen.svg';
import IconRemove from '../assets/icons/icon-trash.svg';
import IconSave from '../assets/icons/icon-disk.svg';


export default {
  name: 'ToDoList',

  components: {
    IconEdit,
    IconRemove,
    IconSave
  },

  data() {
    return {
      newTask: '',
      tasks: []
    }
  },

  methods: {
    addTask() {
      if(this.isEmptyInput) return alert("Es necesario escribir una tarea"); // Validate input

      this.tasks.push({text: this.newTask, editing: false}); // Add element to array
      this.newTask = ''; // Clear input of new task
    },
    removeTask(idx) {
      this.tasks.splice(idx, 1); // Delete element of array with index, one element
    },
    editTask(task) {
      task.editing = true; // Enable input for edit
    },
    saveTask(task) {
      task.editing = false; // Disable input for edit
    }

  },

  computed: {
    isEmptyTasks() { // Check if exist array elements
      return !this.tasks.length;
    },
    isEmptyInput() { // Check if input of newtask is input
      return !this.newTask.length;
    }
  }
}
</script>

<style scoped>
  @import '../assets/styles/todolist.css';
</style>