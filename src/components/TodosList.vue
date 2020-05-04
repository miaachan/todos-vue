<template>
  <div class="mt-2">
    <div class="d-flex justify-content-between">
      <strong
        >{{ headerMsg }} {{ filteredTodos.length }}/{{ todos.length }}</strong
      >
      <strong>Hide</strong>
    </div>

    <ul class="list-unstyled">
      <li
        v-for="todo in filteredTodos"
        :key="todo.id"
        @mouseenter="hoverTodo = todo.id"
        @mouseleave="hoverTodo = null"
      >
        <div class="view p-1 m-2">
          <b-checkbox v-model="todo.completed">
            <label class="title" @dblclick="editTodo(todo)">{{
              todo.title
            }}</label>
          </b-checkbox>

          <transition name="slide-fade">
            <div class="wrapper hide-overflow" v-if="hoverTodo === todo.id">
              <div class="btnwrapper">
                <button class="star" @click="starTodo(todo)"></button>
                <button class="remove" @click="removeTodo(todo)"></button>
              </div>
            </div>
          </transition>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
// eslint-disable-next-line no-unused-vars
let filters = {
  all: function(todos) {
    return todos;
  },
  active: function(todos) {
    return todos.filter(function(todo) {
      return !todo.completed;
    });
  },
  completed: function(todos) {
    return todos.filter(function(todo) {
      return todo.completed && !todo.starred && !todo.secret;
    });
  }
};

let header = {
  active: "Active",
  completed: "Completed"
};

export default {
  name: "TodosList",
  props: {
    todos: Array,
    display: String
  },
  data() {
    return {
      hoverTodo: null
    };
  },
  computed: {
    filteredTodos: function() {
      return filters[this.display](this.todos);
    },
    headerMsg: function() {
      return header[this.display];
    }
  },
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
    removeTodo: function(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1);
    }
  }
};
</script>

<style scoped>
.view {
  display: flex;
  position: relative;
  align-items: center;
  border: 1px solid #f0f0f0;
  overflow: hidden;
}

.wrapper {
  position: absolute;
  display: flex;
  align-items: center;
  right: 0px;
  height: 100%;
  width: 100px;
  overflow: hidden;
}

.btnwrapper {
  display: flex;
  height: 44px;
  width: 100%;
  cursor: pointer;
}

.remove,
.star,
.lock {
  margin-left: auto;
  margin-right: 4px;
  border: 0;
  background-color: inherit;
  font-size: large;
  color: white;
  opacity: 0.3;
  cursor: pointer;
  outline: none;
}

.remove:hover,
.star:hover,
.lock:hover {
  opacity: 1;
  color: #080808;
}

.pressed {
  opacity: 1;
}

.pressed:hover {
  opacity: 0.3;
}

.remove:after {
  content: "❌";
}

.star:after {
  content: "⭐";
}

/* Enter and leave animations can use different */

/* durations and timing functions.              */

.slide-fade-enter-active {
  transition: all 0.5s ease;
}

.slide-fade-leave-active {
  transition: all 0.6s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter,
.slide-fade-leave-to {
  transform: translateX(100px);
  opacity: 0;
}
</style>
