<template>
  <div class="d-flex align-items-center border rounded bg-dark-subtle m-1 p-2 overflow-auto" :class="{ 'border-success-subtle border-1': isDone }">
    <div class="flex-fill overflow-hidden">
      <div class="d-flex flex-column flex-md-row">
        <div class="text-capitalize text-wrap fs-2 text-left me-1 flex-fill d-inline-block text-truncate">
          {{ todoName }}
        </div>
        <p class="text-left text-wrap flex-fill m-1 d-inline-block text-truncate align-self-center" aria-label="Todo Content"> {{ todoContent }}</p>
      </div>
    </div>
    <div class="d-flex justify-content-end flex-row ms-auto">
      <button class="btn btn-danger ms-2" @click="removeTodo" v-show="isDone">
        <i class="bi-trash-fill"></i>
      </button>
      <button class="btn ms-2" :class="{ 'btn-secondary': isDone, 'btn-success': !isDone }" @click="toggleTodo">
        <i class="bi-check-lg" v-show="!isDone"></i>
        <i class="bi-x-lg" v-show="isDone"></i>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  App: 'App',
  props: {
    todoName: {
      type: String,
      required: true,
    },
    todoDone: {
      type: Boolean,
      default: false,
    },
    todoContent: {
      type: String,
      default: "No Description",
    }
  },
  data() {
    return {
      isDone: this.todoDone,
    }
  },
  methods: {
    toggleTodo() {
      this.isDone = !this.isDone;
      this.$emit('toggleTodo');
    },
    removeTodo() {
      this.$emit('removeTodo');
    }
  },
}
</script>

<style>
</style>