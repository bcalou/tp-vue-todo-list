<template>
  <form @submit="addTodo($event)" class="todoNew">
    <input
      placeholder="Do something..."
      v-model="newTodoName"
      class="todoNew__input"
    />
    <button class="todoNew__addButton">Add</button>
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
          editing: false,
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
.todoNew {
  display: flex;
}

.todoNew__input {
  flex: 1;
  height: 40px;
}

.todoNew__addButton {
  padding: 0 1rem;
}
</style>