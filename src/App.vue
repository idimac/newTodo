<template>
<div id="app">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css">
  <h1 class="logo">ToDo List</h1>
  <div class="container bgcont center-block">
    <div class="row justify-content-md-center">
      <div class="col-md-7 col-lg-7 col-sm-12">
      <div class="input-group mb-3">
        <input type="text" class="form-control" v-model="textOfNewTask" @keyup.13="saveNewTask()" placeholder="Enter new task" aria-label="Recipient's username" aria-describedby="basic-addon2">
        <div class="input-group-append">
          <button class="btn btn-outline-secondary" @click="saveNewTask()" type="button">Button</button>
        </div>
      </div>
      <ul class="list-group ">
        <todo v-for="(todo, i) in todos" 
         :key="todo.id" 
         :id ="i" 
         :title="todo.name"
         :completed="todo.completed"
         @save="saveEmitedFromChild" 
         @completedTask="completeEmitedFromChild"
         @remove="removeEmitedFromChild"></todo>
      </ul>
    </div>
  </div>
  </div>
</div>
</template>

<script>
import todotask from "./ToDoTask.vue"
export default {
  name: 'app',
  data () {
    return {
      textOfNewTask: '',
      todos: JSON.parse(localStorage.getItem('myTodos')) || []
    }
  },
  methods: {
      saveNewTask: function () {
      if(this.textOfNewTask) {
      let newTask = {name: this.textOfNewTask, completed: false};
      this.todos.push(newTask);
      localStorage.setItem('myTodos', JSON.stringify(this.todos));
      this.textOfNewTask = "";
      }
    },
      saveEmitedFromChild: function (id, inputNewValue) {
      this.todos[id].edit = !this.todos[id].edit;
      this.todos[id].name = inputNewValue;
    },
      completeEmitedFromChild: function (id) {
        this.todos[id].completed = !this.todos[id].completed;
        localStorage.setItem('myTodos', JSON.stringify(this.todos));
      },
      removeEmitedFromChild: function (id) {
        var answer = confirm('are you sure to delete this task?');
        if (answer) {
          this.todos.splice(id, 1);
          localStorage.setItem('myTodos', JSON.stringify(this.todos));
        }
        
      }
  },
  components: {
    'todo': todotask
  }
}
</script>

<style>
#app {
  text-align: center;
  background-color: rgb(148, 221, 215);
  min-width: 100vw;
  min-height: 100vh;
}
.input-group {
  margin: auto;
  width: 650px !important;
}
.form-control {
  background-color: #c3e7cb !important;
}
.list-group {
  margin: auto !important;
}
.logo {
   font: 29pt Arial;
   color: #a4bea9;
}
</style>
