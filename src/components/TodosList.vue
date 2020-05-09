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

    <transition name="slide-hide">
      <transition-group
        name="fade"
        tag="ul"
        class="todo-list list-unstyled"
        v-if="!hide"
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
              <div class="title" @dblclick="editTodo(todo)">
                {{ todo.title }}
              </div>
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
                  <font-awesome-icon
                    :icon="starIcon"
                    @click="starTodo(todo)"
                    @mouseover="starIcon = ['fas', 'star']"
                    @mouseout="starIcon = ['far', 'star']"
                  />
                  <font-awesome-icon
                    :icon="trashIcon"
                    @click="removeTodo(todo)"
                    @mouseover="trashIcon = ['fas', 'trash-alt']"
                    @mouseout="trashIcon = ['far', 'trash-alt']"
                  />
                </div>
              </div>
            </transition>
          </div>
        </li>
      </transition-group>
    </transition>
  </div>
</template>

<script>
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
      hide: false,
      starIcon: ["far", "star"],
      trashIcon: ["far", "trash-alt"]
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
.test {
  color: black;
}
.light {
  font-size: 0.9em;
  letter-spacing: 2px;
  font-weight: 400;
  color: grey;
}

.slide-hide-enter-active {
  /* transition: all 0.3s ease; */
  transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-hide-leave-active {
  transition: all 0.4s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-hide-enter,
.slide-hide-leave-to {
  transform: translateY(400px);
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
  width: 55px;
}

.btnwrapper {
  display: flex;
  height: 100%;
  width: 100%;
  cursor: pointer;
  align-items: center;
  justify-content: space-between;
  padding-right: 4px;
}

.pressed {
  opacity: 1;
}

.pressed:hover {
  opacity: 0.3;
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
