<template>
  <div id="app">
    <Header />
    <AddTodo 
    v-on:add-todo="addTodoFunc"/>
    <Todos 
    v-bind:list="todos_list"
    v-on:del-todo="deleteTodo"
    />
  </div>
</template>

<script>
import Header from './components/layout/Header'
import Todos from './components/Todos'
import AddTodo from './components/AddTodo'
var jsonPath = require('JSONPath');

export default {
  name: 'app',
  components: {
    Header,
    Todos,
    AddTodo
  },
  data() {
    return {
      todos_list: []
    }
  },
  methods: {
    deleteTodo(id) {
      this.todos_list = this.todos_list.filter(todo => todo.id !== id);
    },
    addTodoFunc(object) {
      this.todos_list.push(object);
    }
  },
  created() {
    fetch("https://jsonplaceholder.typicode.com/todos", {
      "method": "GET"
    })
    .then(response => response.json())
    .then(myJson => {
      console.log(myJson);
      JSONPath(myJson, '$.1');

    })
    .catch(err => {
      console.log(err);
    });
  }
}
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
  }
  .btn {
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;
    cursor: pointer;
  }

  .btn:hover {
    background: #666;
  }
</style>
