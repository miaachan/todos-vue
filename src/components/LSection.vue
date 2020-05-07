<template>
  <div class="LSection">
    Hi, {{ user }}!
    <div class="light">{{ date }}</div>

    <div class="wrapper">
      <div class="w-100 mt-2 mb-2"></div>
    </div>

    <div class="light mb-2">Tag</div>

    <ul class="tags list-unstyled">
      <li
        v-for="tag in this.todoTags"
        :key="tag.id"
        :class="tag"
        class="d-flex align-items-center m-1"
      >
        <label class="mr-3" :class="tag"></label>

        <span>{{ tag.charAt(0).toUpperCase() + tag.slice(1) }}</span>

        <span class="ml-auto light">{{ todosLength(tag) }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "LSection",
  props: {
    user: String,
    todos: Array,
    todoTags: Array
  },
  mounted() {
    // console.log();
  },
  computed: {
    date: () => new Date().toDateString()
  },
  methods: {
    todosLength(tag) {
      return this.todos.filter(todo => todo.tag === tag).length;
    }
  }
};
</script>

<style scoped>
.LSection {
  background-color: #f5f3f6;
  height: 100%;
  border-right: 2px solid #f3f3f3;
}

.w-100 {
  width: 90% !important;
  background-color: #bdc0ba;
  padding: 1px;
}

.tags li label {
  margin: 0;
  padding: 6px;
  border: 2px solid #f0f0f0;
  border-radius: 18px;
  box-shadow: 1px 2px 6px 1px rgba(0, 0, 0, 0.2);
  outline: none;
  transition: padding 0.15s ease-out;
  cursor: pointer;
}

label ~ span {
  font-size: 0.9rem;
}

input:checked ~ label {
  padding: 8px;
  border: 2px solid #f0f0f0;
  border-radius: 18px;
  outline: none;
  /* box-shadow: 2px 2px 6px 0 rgba(0, 0, 0, 0.2); */
}
</style>
