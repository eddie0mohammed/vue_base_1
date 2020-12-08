<template>
  <div id="app">
    <Header />

    <AddTodo 
      v-on:add-todo="addTodo"/>

    <Todos 
      v-bind:todos="todoItems" 
      v-on:del-todo="deleteTodo"
      />

  </div>
</template>

<script>

import Todos from './components/Todos';
import Header from './components/Header';
import AddTodo from './components/AddTodo';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    Todos,
    Header,
    AddTodo,
  },

  data(){
    return {
      todoItems: [
        {
          id: 1,
          title: 'Todo One',
          completed: false
        },
        {
          id: 2,
          title: 'Todo Two',
          completed: false
        },
        {
          id: 3,
          title: 'Todo Three',
          completed: true
        },
      ]
    }
  },
  
  methods: {
    deleteTodo(id){
      const currentTodoItems = this.todoItems;
      const newTotoItems = currentTodoItems.filter(elem => elem.id !== id);
      this.todoItems = newTotoItems;
    },

    addTodo(newTodo) {
      this.todoItems = [...this.todoItems, newTodo];
    }
  },

  // created(){
  //   axios.get('https://jsonplaceholder.typicode.com/todos')
  //     .then(res => console.log(res.data))
  //     .catch(err => console.log(err));
  // }
  async created() {
    try{

      const res = await axios.get('https://jsonplaceholder.typicode.com/todos?_limit=10');
      const todos = res.data;
      this.todoItems = todos;
    }
    catch (err){
      console.log(err);
    }
  }

}
</script>

<style>
*{
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  line-height: 1.4;
}
</style>
