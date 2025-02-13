<template>
  <div>
    <div v-if="!editMode">
      <input type="checkbox" v-model="isChecked" />
      <span>
        <strong>{{ todo.title }}</strong>
      </span>
      <span>
        {{ todo.description }}
      </span>

      <button @click="toggleEditTodo">Edit</button>
      <button @click="removeTodo">Remove</button>
    </div>
    <div v-else>
      <TodoForm @onUpdateTodo="handleUpdateTodo" :todo="todo" isEditMode />
    </div>
  </div>
</template>

<script>
import TodoForm from "./TodoForm.vue";

export default {
  name: "TodoListItem",
  components: { TodoForm },
  props: {
    todo: {
      type: Object,
      required: true,
    },
  },
  computed: {
    isChecked: {
      get() {
        return this.todo.isChecked;
      },
      set(value) {
        this.$emit("onToggleIsChecked", {
          value,
          id: this.todo.id,
        });
      },
    },
    editMode() {
      return this.todo.editMode;
    },
  },

  methods: {
    toggleEditTodo() {
      this.$emit("onToggleEditTodo", this.todo.id);
    },
    handleUpdateTodo(updatedTodo) {
      this.$emit("onUpdateTodo", updatedTodo);
    },
    removeTodo() {
      this.$emit("onRemoveTodo", this.todo.id);
    },
  },
};
</script>

<style></style>
