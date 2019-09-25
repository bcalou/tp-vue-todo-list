<template>
  <li>
    <input type="checkbox" v-model="todo.done" :id="todo.id" />
    <label :for="todo.id">{{ todo.name }}</label>
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
input:checked ~ label {
  text-decoration: line-through;
}
</style>