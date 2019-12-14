<template>
  <li class="todoItem">
    <input type="checkbox" v-model="todo.done" class="todoItem__checkbox" />

    <div v-if="todo.editing">
      <form @submit.prevent="setName()" class="todoItem__edit">
        <input v-model="newTodoName" ref="newNameInput" />
        <button>Edit</button>
      </form>
      <p v-if="inputError" class="error">The todo name cannot be empty</p>
    </div>

    <div v-else @click="enterEditMode()" class="todoItem__name">
      {{ todo.name }}
    </div>
  </li>
</template>

<script lang="ts">
import Vue from 'vue';
import Todo from '@/models/todo';

export default Vue.extend({
  props: {
    todo: Object as () => Todo,
  },
  data(): { newTodoName: string; inputError: boolean } {
    return {
      newTodoName: this.todo.name,
      inputError: false,
    };
  },
  watch: {
    'todo.done'(newValue) {
      this.$store.dispatch('todos/setDone', {
        todo: this.todo,
        done: newValue,
      });
    },
    'newTodoName'(newTodoName) {
      if (newTodoName.length) {
        this.inputError = false;
      }
    },
  },
  methods: {
    enterEditMode(): void {
      if (!this.todo.done) {
        this.$store.dispatch('todos/enterEditMode', { todo: this.todo });

        Vue.nextTick().then(() =>
          (this.$refs.newNameInput as HTMLInputElement).focus(),
        );
      }
    },
    setName(): void {
      if (this.newTodoName.length) {
        this.$store.dispatch('todos/setName', {
          todo: this.todo,
          name: this.newTodoName,
        });
        this.$store.dispatch('todos/quitEditMode');
      } else {
        this.inputError = true;
      }
    },
  },
});
</script>

<style lang="scss" scoped>
.todoItem {
  margin: 1rem 0;
  display: flex;
}

.todoItem__checkbox {
  height: 2rem;
  width: 2rem;
  margin-right: 1rem;
  flex-shrink: 0;

  &:checked ~ .todoItem__name {
    text-decoration: line-through;
  }
}

.todoItem__name {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 100%;
}

.todoItem__edit {
  display: flex;
}
</style>