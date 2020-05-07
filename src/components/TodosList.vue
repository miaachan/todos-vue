<template>
  <div class="mt-2">
    <div class="d-flex justify-content-between align-items-center p-2">
      <strong>{{ headerMsg }}</strong>
      <span class="light mr-auto ml-3 pt-1">
        {{ filteredTodos.length }}/{{ todos.length }}</span
      >
      <span @click="hide = !hide" style="cursor:pointer;">
        <strong v-if="hide == false" @click="hideTodosList()">Hide</strong>
        <strong v-else>Show</strong>
      </span>
    </div>

    <transition-group
      name="fade"
      tag="ul"
      class="todo-list list-unstyled"
      :class="{ 'hide-list': hide }"
    >
      <li
        v-for="todo in filteredTodos"
        :key="todo.id"
        @mouseenter="hoverTodo = todo.id"
        @mouseleave="hoverTodo = null"
      >
        <div class="view p-1 m-2">
          <b-checkbox v-model="todo.completed" @input="toggleTodo(todo)">
          </b-checkbox>
          <div class="todo-title-wrapper">
            <div class="title" @dblclick="editTodo(todo)">{{ todo.title }}</div>
            <div class="w-100"></div>
            <div
              class="d-flex align-items-center"
              v-if="todo.tag != 'all' || todo.completed"
            >
              <label class="mr-4" :class="todo.tag"></label>
              <span class="light" v-if="todo.time">{{ todo.time }}</span>
              <!-- <span class="light" v-if="!todo.time">{{ todo.tag }}</span> -->
            </div>
          </div>

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
    </transition-group>
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
      hoverTodo: null,
      hide: false
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
    // eslint-disable-next-line no-unused-vars
    toggleTodo: function(todo) {
      if (todo.completed) {
        todo.time = new Date().toLocaleString("en-US").replace(",", "");
      } else {
        todo.time = null;
      }
    },
    removeTodo: function(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1);
    },
    hideTodosList: function() {}
  }
};
</script>

<style scoped>
.light {
  font-size: 0.9em;
  letter-spacing: 2px;
  font-weight: 400;
  color: grey;
}

.todo-list {
  transition: all 0.3s ease;
}

.hide-list {
  transform: translateY(400px);
  position: absolute;
  padding: 0 !important;
  margin: 0 !important;
  opacity: 0;
}

.view {
  display: flex;
  position: relative;
  /* align-items: center; */
  /* min-height: 44px; */
  border: 1px solid #f0f0f0;
  overflow-x: hidden;
}

.todo-title-wrapper {
  display: flex;
  flex-wrap: wrap;
}

.title {
  word-break: break-all;
}

.wrapper {
  margin-left: auto;
  height: 24px;
  width: 100px;
}

.btnwrapper {
  display: flex;
  height: 100%;
  width: 100%;
  cursor: pointer;
}

.remove,
.star {
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
.star:hover {
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

.todo-title-wrapper label {
  padding: 6px;
  margin: 0;
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

.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s;
}

.fade-enter,
.fade-leave-to {
  /* color: #d9d9d9; */
  transform: translateX(80vw);
  /* opacity: 0; */
}

.fade-leave-to label.title {
  /* color: #d9d9d9; */
  text-decoration: line-through;
}
</style>
