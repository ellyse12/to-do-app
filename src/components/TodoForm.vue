<template>
  <div class="bg-white rounded-lg shadow-md p-6">
    <form @submit.prevent="submit" class="space-y-4">
      <div>
        <input
          required
          placeholder="Task Title"
          v-model="title"
          class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500 outline-none transition"
        />
      </div>

      <div>
        <input
          required
          placeholder="Task Description"
          v-model="description"
          class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500 outline-none transition"
        />
      </div>

      <button
        type="submit"
        class="w-full bg-emerald-500 text-white py-2 px-4 rounded-lg hover:bg-emerald-600 transition duration-200 font-medium"
      >
        {{ isEditMode ? "Save Changes" : "Add Task" }}
      </button>
    </form>
  </div>
</template>

<script>
export default {
  name: "TodoForm",
  data() {
    return {
      title: "",
      description: "",
    };
  },
  methods: {
    submit() {
      if (this.isEditMode) {
        this.emitUpdateTodo();
        return;
      }
      this.emitNewTodo();
    },
    emitUpdateTodo() {
      alert("Changes done!");
      this.$emit("onUpdateTodo", {
        title: this.title,
        description: this.description,
        id: this.todo.id,
        isChecked: this.todo.isChecked,
        editMode: false,
      });
    },
    emitNewTodo() {
      this.$emit("onNewTodo", {
        title: this.title,
        description: this.description,
        id: `todo_${Math.random() * 10000}`,
        isChecked: false,
        editMode: false,
      });
      this.title = "";
      this.description = "";
    },
  },
  props: {
    todo: {
      type: Object,
      default: () => ({}),
    },
    isEditMode: {
      type: Boolean,
      default: false,
    },
  },
  created() {
    if (this.isEditMode) {
      this.title = this.todo.title;
      this.description = this.todo.description;
      return;
    }
  },
};
</script>

<style></style>
