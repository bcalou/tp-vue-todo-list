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
import { watch, ref } from '@vue/composition-api';

export default Vue.extend({
  setup(props, { root: { $store } }) {
    const newTodoName = ref('');
    const inputError = ref(false);

    function addTodo(): void {
      if (newTodoName.value.length) {
        $store.dispatch('todos/add', { name: newTodoName.value });

        newTodoName.value = '';
      } else {
        inputError.value = true;
      }
    }

    watch(
      () => newTodoName,
      () => {
        if (newTodoName.value.length) {
          inputError.value = false;
        }
      },
    );

    return { newTodoName, inputError, addTodo };
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