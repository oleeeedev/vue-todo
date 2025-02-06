<template>
  <KeepAlive include="TodoComponent">
    <div class="container-fluid d-flex flex-column bg-secondary-subtle vh-100">
      <div class="position-relative">
        <button class="btn btn-lg border position-absolute top-0 end-0" data-bs-toggle="modal" data-bs-target="#modal"
                aria-controls="offcanvas">
          <i class="three-dots bi-three-dots-vertical p-2" aria-label="Settings"></i>
        </button>
        <button class="btn btn-lg btn-success shadow border border-2 border-success-subtle d-block d-md-none bottom-right"
                type="button" data-bs-toggle="offcanvas" data-bs-target="#offcanvas" aria-controls="offcanvas">
          <i class="bi-plus-lg"></i>
          Add
        </button>
      </div>
      <div class="d-block d-md-none fs-1">
        <div class="text-left">
          <i class="bi-alarm"></i> Todo App
          <p class="text-muted fs-4">You have <span style="color: forestgreen">{{ doneTodos.length }}</span>/{{
              todos.length }} Todos done.</p>
        </div>
      </div>
      <div class="text-center d-none d-md-block fs-2">
        <i class="bi-alarm"></i> Todo App
        <p class="text-muted fs-4">You have <span style="color: forestgreen">{{ doneTodos.length }}</span>/{{ todos.length
          }} Todos done.</p>
      </div>

      <form @submit.prevent="addTodo">
        <div class="mb-1 flex-row d-none d-md-flex justify-content-evenly">
          <div class="form-floating flex-fill mb-1 mt-1">
            <input type="text" class="form-control" id="todoName" placeholder="Todo Name" v-model="todoTitle"
                   ref="todoName">
            <label for="todoName">Todo Name</label>
          </div>
          <div class="form-floating flex-fill ms-1 mt-1 mb-1" v-show="contentMode == 'tac'">
            <input type="text" class="form-control" id="todoContent" placeholder="Todo Content" v-model="todoContent">
            <label for="todoContent">Todo Content</label>
          </div>
          <button type="submit" class="btn btn-success text-capitalize m-1" data-bs-toggle="tooltip"
                  title="Add any Todo">Add Todo</button>
        </div>
      </form>
      <hr>
      <div class="offcanvas offcanvas-bottom" tabindex="-1" id="offcanvas">
        <div class="offcanvas-header">
          <h5 class="offcanvas-title">Add Todo</h5>
          <button type="button" class="btn-close text-reset" data-bs-dismiss="offcanvas" aria-label="Close"></button>
        </div>
        <div class="offcanvas-body">
          <form @submit.prevent="addTodo">
            <div class="input-group">
              <input type="text" class="form-control shadow" placeholder="Todo Title" aria-label="Todo Title"
                     aria-describedby="addTodoA" v-model="todoTitle">
              <input type="text" class="form-control shadow" placeholder="Todo Content" aria-label="Todo Content"
                     aria-describedby="addTodoA" v-model="todoContent">
              <button class="btn btn-success shadow" type="submit" id="addTodoB" data-bs-dismiss="offcanvas">Add</button>
            </div>
          </form>
        </div>
      </div>
      <ul class="list p-0 pe-1" v-auto-animate>
        <li class="divider border-bottom bg-light-subtle shadow text-start rounded fs-3 p-2" key="divider-1"
            v-show="notDoneTodos.length != 0 || todos.length == 0">
          <i class="bi-x-lg"></i>
          Not Done Todos
          <span class="badge bg-success rounded-pill ms-2">{{ notDoneTodos.length }}</span>
        </li>
        <li v-for="todo in notDoneTodos" :key="todo">
          <TodoEntry :todoName="todo.name" :todoContent="todo.content" :todoDone="todo.isDone"
                     @toggleTodo="toggleTodo(todo)" @removeTodo="removeTodo(todo)" />
        </li>
        <li class="divider border-bottom bg-light-subtle shadow text-start rounded fs-3 p-2" key="divider-2"
            v-show="doneTodos.length != 0">
          <i class="bi-check-lg"></i>
          Done Todos
          <span class="badge bg-success rounded-pill ms-2">{{ doneTodos.length }}</span>
        </li>
        <li v-for="todo in doneTodos" :key="todo">
          <TodoEntry :todoName="todo.name" :todoContent="todo.content" :todoDone="todo.isDone"
                     @toggleTodo="toggleTodo(todo)" @removeTodo="removeTodo(todo)" />
        </li>
      </ul>
    </div>
  </KeepAlive>

  <div class="modal fade" id="modal" tabindex="-1" aria-labelledby="modalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="modalLabel">
            <i class="bi-gear"></i>
            Settings
          </h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <div class="mb-1 d-flex align-items-center">
            <h5>Notes Mode</h5>
            <p class="text-muted badge small">(Click to Toggle)</p>
            <div v-auto-animate class="d-flex justify-content-end flex-row ms-auto">
              <button class="btn btn-success ms-auto" v-if="contentMode === 'tac'" @click="contentMode = 'to'">Title and
                Content</button>
              <button class="btn btn-success ms-auto" v-else-if="contentMode === 'to'" @click="contentMode = 'tac'">Title
                Only</button>
            </div>
          </div>
          <hr>
          <div class="mb-1 d-flex flex-row">
            <h5>Theme</h5>
            <p class="text-muted badge small">(Click to Toggle)</p>
            <div v-auto-animate class="d-flex justify-content-end flex-row ms-auto">
              <button class="btn btn-success me-auto" v-if="darkMode" @click="toggleDarkMode">Dark Mode</button>
              <button class="btn btn-success me-auto" v-else @click="toggleDarkMode">White Mode</button>
            </div>
          </div>
        </div>
        <div class="modal-footer bg-secondary-subtle">
          <p>Created by <a href="https://street14.work/home">Alex</a> with ♥️</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Transition } from 'vue';
import TodoEntry from './TodoEntry.vue';

export default {
  name: 'App',
  components: {
    TodoEntry,
    Transition
  },
  mounted() {
    this.$refs.todoName.focus();
    this.loadTodos();
  },
  data() {
    return {
      todoTitle: "",
      todoContent: "",
      todos: [],
      currentMode: 'add',
      contentMode: "to",
      darkMode: false
    }
  },
  watch: {
    todos: {
      handler() {
        this.saveTodos();
      },
      deep: true
    }
  },
  methods: {
    toggleDarkMode() {
      this.darkMode = !this.darkMode;

      //Change data-bs-theme
      document.documentElement.setAttribute('data-bs-theme', this.darkMode ? 'dark' : 'light');
    },
    addTodo() {
      if (!this.todoTitle && (!this.todoContent || this.contentMode)) return;

      this.todos.unshift({
        name: this.todoTitle.trim(),
        content: this.todoContent.trim(),
        isDone: false
      });

      //Reset input fields
      this.todoTitle = "";
      this.todoContent = "";
    },
    async saveTodos() {
      localStorage.setItem('savedTodos', JSON.stringify(this.todos));
    },
    loadTodos() {
      let lTodos = localStorage.getItem('savedTodos');
      this.todos = lTodos ? JSON.parse(lTodos) : [];
    },
    toggleTodo(todo) {
      //Change property of todo: todoDone = !todoDone
      todo.isDone = !todo.isDone;
    },
    removeTodo(todo) {
      //Remove todo from todos
      this.todos.splice(this.todos.indexOf(todo), 1);
    }
  },
  computed: {
    doneTodos() {
      return this.todos.filter((todo) => todo.isDone);
    },
    notDoneTodos() {
      return this.todos.filter((todo) => !todo.isDone);
    }
  }
}
</script>

<style scoped>
.list-move,
  /* apply transition to moving elements */
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

/* ensure leaving items are taken out of layout flow so that moving
   animations can be calculated correctly. */
.list-leave-active {
  position: absolute;
}

.list::-webkit-scrollbar {
  width: 0.5rem;
}

.list::-webkit-scrollbar-track {
  background: var(--bs-secondary);
}

.list::-webkit-scrollbar-thumb {
  background: var(--bs-dark);
}

.list::-webkit-scrollbar-thumb:hover {
  background: var(--bs-dark);
}

.list {
  list-style: none;
  margin: 0;
  overflow-y: scroll;
  padding: 0;

}

.divider {
  position: sticky;
  top: 0;
  background: transparent;
  text-align: left;
  padding: 8px;
  background-color: var(--bs-secondary);
  text-align: center;
}

.bottom-right {
  position: fixed;
  bottom: 25px;
  right: 20px;
  z-index: 99;
}

.addTodo {
  display: flex;
}

.wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: clamp(300px, 100%, 600px);
  min-height: 100%;
}
</style>