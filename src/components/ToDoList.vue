<template>
<div :class="{ 'dark-mode': isDarkMode }">
  <div class="toggle-container">
      <span>🌞</span>
      <label class="switch">
        <input type="checkbox" v-model="isDarkMode" />
        <span class="slider"></span>
      </label>
      <span>🌜</span>
    </div>
  <div class="todo-container">
    <h1>To-Do List</h1>
    <div class="hello">
        Hello, <input v-model="userName" placeholder="Enter your name" class="name-input"/>
    </div>
    <div class="input-container">
    <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add task"/>
    <select v-model="newPriority">
      <option value="low">Low Priority</option>
      <option value="medium">Normal Priority</option> 
      <option value="high">Urgent</option>
    </select>
    <button @click="addTodo">Add</button>
    </div>
    <ul class="todo-list">
      <li v-for="(todo, index) in todos" :key="index" :class="{ done: todo.completed, [todo.priority]: true }">
        <div class="todo-item">
            <input type="checkbox" v-model="todo.completed" />
            <span :class="{ done: todo.completed }">{{ todo.text }}</span>
        </div>
        <button @click="deleteTodo(index)">Delete</button>
      </li>
    </ul>
  </div>
</div>
</template>

<script>
import '../assets/styles/TodoList.css';
import '../assets/styles/darkMode.css';
export default {
  data() {
    return {
      isDarkMode: false,
      userName: this.loadUserName(),
      newTodo: '',
      newPriority: 'low',
      todos: this.loadTodos()
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim()) {
        this.todos.push({ text: this.newTodo, completed: false, priority: this.newPriority });
        this.newTodo = '';
        this.newPriority = 'low';
        this.saveTodos();
      }
    },
    deleteTodo(index) {
      this.todos.splice(index, 1);
      this.saveTodos();
    },
    saveTodos() {
        localStorage.setItem('todos', JSON.stringify(this.todos));
    },
    loadTodos(){
        const savedTodos = localStorage.getItem('todos');
        return savedTodos ? JSON.parse(savedTodos) : [];
    },
    loadUserName() {
        const savedName = localStorage.getItem('userName');
        return savedName ? savedName : '';
    },
    toggleDarkMode() {
      this.isDarkMode = !this.isDarkMode;
    }
  },
  watch: {
    userName(newName) {
        localStorage.setItem('userName', newName);
    },
    todos: {
        handler() {
            this.saveTodos();
        },
        deep: true
    }
  }
};
</script>
