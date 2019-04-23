<template>
  <div>
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
/* eslint-disable */
import Todos from '../components/Todos';
import AddTodo from '../components/AddTodo';
import axios from 'axios';
import uuid from 'uuid/v1';

export default {
  name: 'Home',
  components: {
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: [],
      newTodo: null
    }
  },
  mounted() {
    console.log('Mounting local storage...')
    if (localStorage.getItem('mystore')) {
      console.log('Mounting mystore')
      try {
        this.mystore = JSON.parse(localStorage.getItem('mystore'));
      } catch(e) {
        localStorage.removeItem('mystore');
      }
    }
  },
  methods: {
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
      this.saveTodos()
    },
    addTodo(newTodo) {
    const { title, completed } = newTodo;
      if (this.todos) {
      this.todos.unshift(
        {
          id: uuid(),
          title,
          completed
        })
      } else {
        this.todos = [
          {
            id: uuid(),
            title,
            completed
          }
        ]
      }
      this.saveTodos()
    },
    saveTodos() {
      const parsed = JSON.stringify(this.todos);
      localStorage.setItem('mystore', parsed);
    }
  },
  created() {
    try {
      this.todos = JSON.parse(localStorage.getItem('mystore'));
    } catch(e) {
      localStorage.removeItem('mystore');
    }
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
