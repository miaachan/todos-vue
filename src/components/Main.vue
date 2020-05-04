<template>
  <div class="Main">
    <h4>Todos List</h4>
    <b-form inline>
      <b-input
        v-model="newTodo"
        placeholder="Enter your todos"
        @keyup.enter="addTodo()"
      ></b-input>

      <b-button variant="outline-primary" class="ml-2" @click="addTodo()"
        >Save</b-button
      >
    </b-form>

    <TodosList :todos="todos" display="active" />

    <TodosList :todos="todos" display="completed" />
  </div>
</template>

<script>
import TodosList from "./TodosList";

export default {
  components: {
    TodosList
  },
  props: {},
  data() {
    return {
      todoStorage: {
        STORAGE_KEY: "todos-vuejs",
        uid: 0
      },
      todos: [],
      newTodo: "",
      hoverTodo: ""
    };
  },
  created: function() {
    this.todos = this.fetch();
  },
  watch: {
    todos: {
      handler: function(todos) {
        this.save(todos);
      },
      deep: true
    }
  },
  computed: {},

  methods: {
    fetch: function() {
      let todos = JSON.parse(
        localStorage.getItem(this.todoStorage.STORAGE_KEY) || "[]"
      );
      todos.forEach(function(todo, index) {
        todo.id = index;
      });
      this.todoStorage.uid = todos.length;
      return todos;
    },
    save: function(todos) {
      localStorage.setItem(this.todoStorage.STORAGE_KEY, JSON.stringify(todos));
    },
    addTodo: function() {
      let value = this.newTodo && this.newTodo.trim();
      if (!value) return;
      this.todos.push({
        // id: todoStorage.uid++,
        title: value,
        completed: false,
        tag: false
      });
      this.newTodo = "";
    }
  }
};
</script>

<style scoped>
.Main {
  background-color: #fefefe;
  height: 100%;
}

.new-todo {
  position: relative;
  flex: 1 1 100%;
  margin: 0;
  width: 100%;
  font-size: 24px;
  font-family: inherit;
  font-weight: inherit;
  line-height: 1.4em;
  color: inherit;
  box-sizing: border-box;
  outline: none;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding: 16px 16px 16px 60px;
  border: none;
  background: rgba(0, 0, 0, 0.003);
  box-shadow: inset 0 -2px 1px rgba(0, 0, 0, 0.03);
}
</style>
