<template>
  <div id="app">
    <h1>Todo</h1>
    <input type="text" v-model="todoName" @keyup.enter="addTodo" />
    <ul>
      <li v-for="todo of todos" :key="todo.id">
        <td>{{ todo.name }}</td>
        <td><Button v-on:click="deleteUser(todo.id)">Delete</Button></td>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

const baseURL = "http://localhost:3000/todos";

export default {
  name: "App",
  data() {
    return {
      todos: [],
      todoname: "",
    };
  },
  async created() {
    try {
      const res = await axios.get(baseURL);

      this.todos = res.data;
    } catch (e) {
      console.error(e);
    }
  },
  methods: {
    async addTodo() {
      const res = await axios.post(baseURL, { name: this.todoName });

      this.todos = [...this.todos, res.data];

      this.todoName = "";
    },

    getUsers() {
      axios.get("http://localhost:3000/todos").then((result) => {
        console.warn(result);
        this.todos = result.data;
      });
    },
    deleteUser(id) {
      axios.delete("http://localhost:3000/todos/" + id).then((result) => {
        console.warn(result);
        this.getUsers();
      });
    },
  },
  mounted() {
    this.getUsers();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

ul {
  margin: 0;
  padding: 0;
}

li {
  list-style: none;
}
</style>
