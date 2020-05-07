<template>
  <b-container class="layout">
    <b-row style="min-height:400px;">
      <b-col cols="3" class="p-0">
        <LSection
          class="p-3"
          user="miaachan"
          :todoTags="todoTags"
          :todos="todos"
        />
      </b-col>
      <b-col cols="9" class="p-0">
        <div class="Main p-3">
          <h4>Todos List</h4>
          <!-- <b-form inline @submit.prevent="submit"> -->
          <div class="d-flex">
            <b-input
            class="new-todo"
              v-model="newTodo"
              placeholder="Enter your todos"
              @keyup.enter="addTodo()"
            ></b-input>
            <b-button variant="outline-primary" class="ml-2" @click="addTodo()">
              Save
            </b-button>
          </div>

          <!-- </b-form> -->

          <div class="newtodo-tags mt-2">
            <div class="d-inline-block mr-1" v-for="tag in todoTags" :key="tag">
              <input
                type="radio"
                v-model="newTodoTag"
                :id="tag"
                :value="tag"
                :class="tag"
              />
              <label :for="tag" :class="tag"></label>
            </div>
          </div>

          <TodosList :todos="todos" display="active" />

          <TodosList :todos="todos" display="completed" />

          <div class="wrapper">
            <span @click="clearAll()" class="light" style="cursor: pointer;"
              >clear all</span
            >
          </div>
        </div>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import LSection from "./LSection";
import TodosList from "./TodosList";

export default {
  components: {
    LSection,
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
      todoTags: ["all", "red", "orange", "yellow", "green", "blue", "purple"],
      newTodo: "",
      newTodoTag: "all",
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
        id: this.todoStorage.uid++,
        title: value,
        completed: false,
        tag: this.newTodoTag
      });
      this.newTodo = "";
      // this.newTodoTag = "all";
    },
    clearAll: function() {
      this.todos = [];
    }
  }
};
</script>

<style scoped>
.layout {
  min-height: 400px;
  max-height: 80vh;
  overflow: hidden;
  box-shadow: 0 5px 8px 4px rgba(0, 0, 0, 0.2);
}
.Main {
  background-color: #fefefe;
  max-height: 80vh;
  height: 100%;
  overflow-x: hidden;
  overflow-y: scroll;
}

.Main::-webkit-scrollbar {
  width: 0 !important;
}

.new-todo {
  flex: 1 1 100%;
  margin: 0;
  font-size: 20px;
  font-weight: inherit;
  line-height: 1.4em;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding: 16px 16px 16px 6px;
  border: none;
  background: rgba(0, 0, 0, 0.003);
  box-shadow: inset 0 -2px 1px rgba(0, 0, 0, 0.03);
}

.newtodo-tags {
  display: flex;
  align-items: center;
  height: 35px;
}

.newtodo-tags input {
  display: none;
}

input ~ label {
  padding: 6px;
  border: 2px solid #f0f0f0;
  border-radius: 18px;
  outline: none;
  transition: padding 0.15s ease-out;
  cursor: pointer;
}

input:checked ~ label {
  padding: 8px;
  border: 2px solid #f0f0f0;
  border-radius: 18px;
  outline: none;
  /* box-shadow: 2px 2px 6px 0 rgba(0, 0, 0, 0.2); */
}

.expand-enter-active {
  transition: all 0.5s ease;
}

.expand-leave-active {
  transition: all 0.6s cubic-bezier(1, 0.5, 0.8, 1);
}

.expand-enter,
.expand-leave-to {
  transform: translateX(100px);
  opacity: 0;
}

.toggle:checked ~ .mark {
  border-color: transparent;
  background: #6871f1;
  animation: jelly 0.6s ease;
}

.toggle:checked ~ .mark {
  border-color: transparent;
  background: #6871f1;
  animation: jelly 0.6s ease;
}
</style>


<style>
label.all {
  /* background-color: blue; */
}

label.red {
  background-color: #ee5b65;
}
label.orange {
  background-color: #f49c59;
}
label.yellow {
  background-color: #fff59d;
}

label.green {
  background-color: #72bc6a;
}

label.blue {
  background-color: #3cafe7;
}

label.purple {
  background-color: #be82c6;
}

.heavy {
  font-size: 0.9em;
  letter-spacing: 2px;
  font-weight: 100;
  color: grey;
}

.light {
  font-size: 0.9em;
  letter-spacing: 2px;
  font-weight: 100;
  color: grey;
}

.wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>