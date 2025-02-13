<template>
  <div class="bg-gray-50 rounded-lg p-4 border border-gray-200">
    <div v-if="!editMode" class="flex items-center justify-between">
      <div class="flex items-center space-x-3">
        <input
          type="checkbox"
          v-model="isChecked"
          class="w-5 h-5 text-emerald-500 rounded border-gray-300 focus:ring-emerald-500"
        />
        <div class="flex flex-col">
          <strong
            :class="{ 'line-through text-gray-500': isChecked }"
            class="text-gray-800"
          >
            {{ todo.title }}
          </strong>
          <span
            :class="{ 'line-through text-gray-400': isChecked }"
            class="text-gray-600 text-sm"
          >
            {{ todo.description }}
          </span>
        </div>
      </div>

      <div class="flex space-x-2">
        <button
          @click="toggleEditTodo"
          class="px-3 py-1 text-sm flex justify-center bg-teal-500 text-white rounded-md hover:bg-cyan-600 transition min-w-12"
        >
          <img src="../assets/pencil.svg" alt="" class="h-4 w-4" />
        </button>
        <button
          @click="removeTodo"
          class="px-3 py-1 text-sm flex justify-center bg-indigo-400 text-white rounded-md hover:bg-red-400 transition min-w-12"
        >
          <img src="../assets/trash-can.svg" alt="" class="h-4 w-4" />
        </button>
      </div>
    </div>
    <div v-else>
      <TodoForm
        @onUpdateTodo="handleUpdateTodo"
        :todo="todo"
        :isEditMode="true"
      />
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
