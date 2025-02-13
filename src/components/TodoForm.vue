<template>
  <form @submit.prevent="submit">
    <input required placeholder="title" v-model="title" class="bg-slate-900" />
    <input required placeholder="description" v-model="description" />

    <button type="submit">{{ isEditMode ? "Save" : "Add" }}</button>
  </form>
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
      alert("he");
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
