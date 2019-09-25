<template>
  <li class="todoItem">
    <input
      type="checkbox"
      v-model="todo.done"
      :id="todo.id"
      class="todoItem__checkbox"
    />
    <label :for="todo.id" class="todoItem__label">{{ todo.name }}</label>
  </li>
</template>

<script lang="ts">
import Vue from 'vue';
import Todo from '@/models/todo';

export default Vue.extend({
  props: {
    todo: Object as () => Todo,
  },
  watch: {
    'todo.done'() {
      this.$emit('check');
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

  &:checked ~ .todoItem__label {
    text-decoration: line-through;
  }
}

.todoItem__label {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 100%;
}
</style>