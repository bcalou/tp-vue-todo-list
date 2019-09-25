<template>
  <main>
    <todo-new v-on:newTodo="addTodo($event)"></todo-new>

    <ul>
      <li v-for="todo in todos" :key="todo.id">
        <input type="checkbox" v-model="todo.done" :id="todo.id" />
        <label :for="todo.id">{{ todo.name }}</label>
      </li>
    </ul>

    <button
      v-if="todos.length"
      @click="removeDoneTodos()"
      :disabled="doneTodos.length === 0"
    >
      Remove done todos
    </button>
  </main>
</template>

<script lang="ts">
import Vue from 'vue';
import Todo from '@/models/todo';
import TodoNew from '@/components/TodoNew.vue';

export default Vue.extend({
  components: {
    TodoNew,
  },
  data(): { todos: Todo[] } {
    return {
      todos: [
        {
          id: 1,
          name: 'Manger',
          done: true,
        },
        {
          id: 2,
          name: 'Boire',
          done: false,
        },
      ],
    };
  },
  computed: {
    doneTodos(): Todo[] {
      return this.todos.filter((todo) => todo.done);
    },
  },
  methods: {
    addTodo(todo: Todo): void {
      this.todos.push(todo);
    },
    removeDoneTodos(): void {
      this.todos = this.todos.filter((todo) => !todo.done);
    },
  },
});
</script>

<style scoped lang="scss">
input:checked ~ label {
  text-decoration: line-through;
}
</style>