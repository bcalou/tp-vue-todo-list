<template>
  <main class="todoList">
    <todo-new v-on:newTodo="addTodo($event)"></todo-new>

    <ul class="todoList__items">
      <todo-item
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        v-on:update="saveTodos()"
      ></todo-item>
    </ul>

    <button
      v-if="todos.length"
      @click="removeDoneTodos()"
      :disabled="doneTodos.length === 0"
      class="todoList__removeDone"
    >
      Remove done todos
    </button>
  </main>
</template>

<script lang="ts">
import Vue from 'vue';
import Todo from '@/models/todo';
import TodoNew from '@/components/TodoNew.vue';
import TodoItem from '@/components/TodoItem.vue';

export default Vue.extend({
  components: {
    TodoNew,
    TodoItem,
  },
  data(): { todos: Todo[] } {
    return {
      todos: [],
    };
  },
  computed: {
    doneTodos(): Todo[] {
      return this.todos.filter((todo) => todo.done);
    },
  },
  created() {
    const savedTodos: string | null = localStorage.getItem('todos');

    if (savedTodos) {
      this.todos = JSON.parse(savedTodos);
    }
  },
  methods: {
    addTodo(todo: Todo): void {
      this.todos.push(todo);
      this.saveTodos();
    },
    removeDoneTodos(): void {
      this.todos = this.todos.filter((todo) => !todo.done);
      this.saveTodos();
    },
    saveTodos(): void {
      localStorage.setItem(
        'todos',
        JSON.stringify(this.todos.map((todo) => ({ ...todo, editing: false }))),
      );
    },
  },
});
</script>

<style scoped lang="scss">
.todoList {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}

.todoList__items {
  margin-top: 1rem;
  padding-bottom: 3rem;
}

.todoList__removeDone {
  position: fixed;
  bottom: 0;
  width: 100%;
  height: 3rem;
  left: 0;
  box-shadow: 0 -10px 10px white;
}
</style>