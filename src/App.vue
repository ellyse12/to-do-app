<template>
  <div id="app" class="min-h-screen bg-gray-100 relative">
    <div class="fixed left-0 top-40 h-full w-64 pointer-events-none">
      <img
        src="@/assets/leaf-right.svg"
        alt="Left flower"
        class="h-full object-contain opacity-30"
      />
    </div>

    <div class="fixed right-0 -top-12 h-full w-64 pointer-events-none">
      <img
        src="@/assets/leaf-left.svg"
        alt="Right flower"
        class="h-full object-contain opacity-30"
      />
    </div>

    <h1
      class="text-yellow-50 text-center text-6xl font-semibold bg-emerald-500 py-4 shadow-md relative z-10"
    >
      To-do List
    </h1>
    <div class="container mx-auto px-4 py-8 relative z-10">
      <p class="text-xl font-medium text-gray-700 text-center mb-8">
        Completed To-do's:
        <span class="text-emerald-600"
          >{{ completedTodos }} / {{ todoList.length }}</span
        >
      </p>

      <div class="max-w-2xl mx-auto mb-8">
        <TodoForm @onNewTodo="handleNewTodo" />
      </div>

      <div class="max-w-2xl mx-auto bg-white rounded-lg shadow-md p-6">
        <h2 class="text-2xl font-semibold text-gray-800 mb-4">Your Tasks</h2>
        <div class="space-y-3">
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
      </div>
    </div>
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
      this.saveTodosToLocalStorage();
    },
    handleToggleIsChecked({ value, id }) {
      const todo = this.findTodo(id);
      todo.isChecked = value;
      this.saveTodosToLocalStorage();
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
      this.saveTodosToLocalStorage();
    },
    handleRemoveTodo(id) {
      this.todoList = this.todoList.filter((todo) => todo.id !== id);
      this.saveTodosToLocalStorage();
    },
    saveTodosToLocalStorage() {
      localStorage.setItem("todos", JSON.stringify(this.todoList));
    },
    loadTodosFromLocalStorage() {
      const savedTodos = localStorage.getItem("todos");
      if (savedTodos) {
        this.todoList = JSON.parse(savedTodos);
      }
    },
  },
  created() {
    this.loadTodosFromLocalStorage();
  },
};
</script>

<style>
.pointer-events-none {
  pointer-events: none;
}
</style>
