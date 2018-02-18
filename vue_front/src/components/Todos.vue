<template>
  <div class="todos">
    <h1>{{ message }}</h1>
    <div>
      <div class='new-todo'>
        <span>Title: </span>
        <input
          v-model="newTodo.title"
          type="text"
        />
        <span>Created_by: </span>
        <input
          v-model="newTodo.created_by"
          type="text"
        />
        <button v-on:click="createTodo(newTodo)">new</button>
      </div>
      <ul>
        <li v-for='(todo, index) in todos' v-bind:key="todo.id">
          <div v-if="editTodo === todo.id">
            <input
              v-on:keyup.13="updateTodo(todo)"
              v-model='todo.title'
              type="text"
            />
            <button v-on:click='updateTodo(todo)'>save</button>
          </div>
          <div v-else>
            {{todo.title}} | {{todo.created_by}}
            <button v-on:click="editTodo = todo.id">edit</button>
            <button v-on:click="deleteTodo(todo.id, index)">x</button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
const apiURL = 'http://localhost:3000/todos/';

export default {
  name: 'Todos',
  data() {
    return {
      message: 'List of todos',
      todos: [],
      editTodo: null,
      newTodo: {
        title: null,
        created_by: null,
      },
    };
  },
  mounted() {
    fetch(apiURL)
      .then(response => response.json())
      .then((data) => {
        this.todos = data;
      });
  },
  methods: {
    createTodo(todo) {
      const req = {
        title: todo.title,
        created_by: todo.created_by,
      };
      fetch(apiURL, {
        body: JSON.stringify(req),
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
      })
        .then(response => response.json())
        .then((data) => {
          this.todos.push(data);
          this.newTodo = {
            title: null,
            created_by: null,
          };
        });
    },
    updateTodo(todo) {
      fetch(apiURL + todo.id, {
        body: JSON.stringify(todo),
        method: 'PATCH',
        headers: {
          'Content-Type': 'application/json',
        },
      })
        .then(() => {
          this.editTodo = null;
        });
    },
    deleteTodo(id, index) {
      fetch(apiURL + id, {
        method: 'DELETE',
      })
        .then(() => {
          this.todos.splice(index, 1);
        });
    },
  },
};
</script>

<style scoped>
  h1 {
    font-size: 16px;
  }
  ul {
    list-style: none;
  }
  li {
    border: aquamarine 1px solid;
    background-color: aliceblue;
    border-radius: 4px;
    padding: 4px 8px;
    margin: 5px;
  }
</style>
