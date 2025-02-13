<template>
  <div id="app">
    <h1
      class="text-cyan-700 text-center text-6xl font-semibold bg-emerald-500 py-4"
    >
      To-do List
    </h1>
    <p class="py-4 text-center">
      Completed To-do's : {{ completedTodos }} / {{ todoList.length }}
    </p>
    <TodoForm @onNewTodo="handleNewTodo" />
    <TodoListItem
      v-for="todo in todoList"
      :key="todo.id"
      :todo="todo"
      @onToggleIsChecked="handleToggleIsChecked"
      @onToggleEditTodo="handleToggleEditTodo"
      @onUpdateTodo="handleUpdateTodo"
      @onRemoveTodo="handleRemoveTodo"
    />
  </div>
</template>

<script>
import TodoForm from "./components/TodoForm.vue";
import TodoListItem from "./components/TodoListItem.vue";

export default {
  name: "App",
  components: { TodoForm, TodoListItem },
  data() {
    return {
      todoList: [],
    };
  },
  computed: {
    completedTodos() {
      return this.todoList.filter((todo) => todo.isChecked).length;
    },
  },
  methods: {
    handleNewTodo(newTodo) {
      this.todoList.push(newTodo);
      console.log(this.todoList);
    },
    handleToggleIsChecked({ value, id }) {
      const todo = this.findTodo(id);
      todo.isChecked = value;

      console.log(this.todoList);
    },
    findTodo(id) {
      return this.todoList.find((todo) => todo.id === id);
    },
    handleToggleEditTodo(id) {
      const todo = this.findTodo(id);
      todo.editMode = !todo.editMode;
    },
    handleUpdateTodo(updatedTodo) {
      this.todoList = this.todoList.map((todo) => {
        if (todo.id === updatedTodo.id) {
          return updatedTodo;
        }
        return todo;
      });
    },
    handleRemoveTodo(id) {
      this.todoList = this.todoList.filter((todo) => todo.id !== id);
    },
  },
};
</script>

<style></style>
