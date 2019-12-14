<template>
  <div>
    <form @submit.prevent="addTodo()" class="todoNew">
      <input
        placeholder="Do something..."
        v-model="newTodoName"
        class="todoNew__input"
      />
      <button class="todoNew__addButton">Add</button>
    </form>
    <p v-if="inputError" class="error">The todo name cannot be empty</p>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  data(): { newTodoName: string; inputError: boolean } {
    return {
      newTodoName: '',
      inputError: false,
    };
  },
  methods: {
    addTodo(): void {
      if (this.newTodoName.length) {
        this.$store.dispatch('todos/add', { name: this.newTodoName });

        this.newTodoName = '';
      } else {
        this.inputError = true;
      }
    },
  },
  watch: {
    newTodoName(newTodoName) {
      if (newTodoName.length) {
        this.inputError = false;
      }
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