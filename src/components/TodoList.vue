<template>
  <main>
    <form @submit="addTodo($event)">
      <input placeholder="Do something..." v-model="newTodoName" />
      <button>Add</button>
    </form>

    <ul>
      <li v-for="todo in todos" :key="todo.id">
        <input type="checkbox" v-model="todo.done" :id="todo.id" />
        <label :for="todo.id">{{ todo.name }}</label>
      </li>
    </ul>
  </main>
</template>

<script lang="ts">
import Vue from 'vue';
import Todo from '@/models/todo';

export default Vue.extend({
  data(): { newTodoName: ''; todos: Todo[] } {
    return {
      newTodoName: '',
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
  methods: {
    addTodo(event: Event): void {
      event.preventDefault();

      if (this.newTodoName.length) {
        this.todos.push({
          id: this.getRandomId(),
          name: this.newTodoName,
          done: false,
        });

        this.newTodoName = '';
      }
    },
    getRandomId(): number {
      return parseInt(
        Math.random()
          .toString()
          .substring(2, 12),
        10,
      );
    },
  },
});
</script>

<style scoped lang="scss">
input:checked ~ label {
  text-decoration: line-through;
}
</style>