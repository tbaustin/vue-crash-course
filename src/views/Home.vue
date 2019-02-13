<template>
  <div id="app">
    <AddTodo @add-todo="addTodo"/>
    <Todos v-bind:todos="todos" @del-todo="deleteTodo"/>
  </div>
</template>

<script>
import axios from "axios";

import Todos from "../components/Todos";
import AddTodo from "../components/AddTodo";

export default {
  name: "Home",
  components: {
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    async deleteTodo(id) {
      const res = await axios.delete(
        `https://jsonplaceholder.typicode.com/todos/${id}`
      );
      if (res) {
        this.todos = this.todos.filter(todo => todo.id !== id);
      }
    },
    async addTodo(newTodo) {
      const { title, completed } = newTodo;
      const res = await axios.post(
        `https://jsonplaceholder.typicode.com/todos`,
        {
          title,
          completed
        }
      );
      const { data } = res;
      this.todos = [...this.todos, data];
    }
  },
  async created() {
    const res = await axios.get(
      `https://jsonplaceholder.typicode.com/todos?_limit=10`
    );
    const { data } = res;
    this.todos = data;
  }
};
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

