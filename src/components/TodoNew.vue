<template>
  <form @submit="addTodo($event)">
    <input placeholder="Do something..." v-model="newTodoName" />
    <button>Add</button>
  </form>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  data(): { newTodoName: string } {
    return {
      newTodoName: '',
    };
  },
  methods: {
    addTodo(event: Event): void {
      event.preventDefault();

      if (this.newTodoName.length) {
        this.$emit('newTodo', {
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

<style lang="scss" scoped>
</style>