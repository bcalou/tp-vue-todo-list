<template>
  <div>
    <header>
      <h1>Todo list</h1>
    </header>

    <todo-list></todo-list>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import TodoList from '@/components/TodoList.vue';

export default Vue.extend({
  name: 'home',
  components: {
    TodoList,
  },
});
</script>
