<template>
  <main>
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

export default Vue.extend({
  data() {
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
});
</script>

<style scoped lang="scss">
input:checked ~ label {
  text-decoration: line-through;
}
</style>