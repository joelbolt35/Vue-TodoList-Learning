<template>
  <div id="app">
    <AddTodo 
    v-on:add-todo="addTodoFunc"/>
    <Todos 
    v-bind:list="todos_list"
    v-on:del-todo="deleteTodo"
    />
  </div>
</template>

<script>
import Todos from '../components/Todos'
import AddTodo from '../components/AddTodo'
import jsonPath from 'jsonpath'
// import _ from 'underscore'
import axios from 'axios'

export default {
  name: 'Home',
  components: {
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
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
      .then(() => this.todos_list = this.todos_list.filter(todo => todo.id !== id))
      .catch(err => console.log(err))
    },
    addTodoFunc(object) {
      const { title, completed } = object;
      axios.post("https://jsonplaceholder.typicode.com/todos", {
        title,
        completed
      })
      .then(response => this.todos_list.push(response.data))
      .catch(err => console.log(err));

    }
  },
  created() {
    axios.get('https://jsonplaceholder.typicode.com/todos')
    .then(res => {
      let myJson = res.data;
      let result = [];
      result = result.concat(jsonPath.query(myJson, `$[0:200:50]`));
      // result = result.concat(jsonPath.query(myJson, `$[0]`));
      // result = result.concat(jsonPath.query(myJson, `$[3]`));
      // result = result.concat(jsonPath.query(myJson, `$[2]`));
      // result = result.concat(jsonPath.query(myJson, `$[6]`));
      // result = _.uniq(result, 'id');
      // var sorted = result.sort(function IHaveAName(a, b) { // non-anonymous as you ordered...
      //     return b.id < a.id ?  1 // if b should come earlier, push a to end
      //         : b.id > a.id ? -1 // if b should come later, push a to begin
      //         : 0;                   // a and b are equal
      // });
      // console.log(sorted);
      result.forEach(element => {
        element.completed = false;
      });
      this.todos_list = result;
    }).catch(err => console.log(err));

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
