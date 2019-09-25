<template>
  <li class="todoItem">
    <input type="checkbox" v-model="todo.done" class="todoItem__checkbox" />

    <form @submit="setName($event)" v-if="todo.editing" class="todoItem__edit">
      <input v-model="newTodoName" ref="newNameInput" />
      <button>Edit</button>
    </form>

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
  data(): { newTodoName: string } {
    return {
      newTodoName: this.todo.name,
    };
  },
  watch: {
    'todo.done'() {
      this.$emit('update');
    },
  },
  methods: {
    enterEditMode(): void {
      if (!this.todo.done) {
        this.todo.editing = true;
        Vue.nextTick().then(() =>
          (this.$refs.newNameInput as HTMLInputElement).focus(),
        );
      }
    },
    setName(event: Event): void {
      event.preventDefault();
      this.todo.editing = false;

      if (this.newTodoName.length) {
        this.todo.name = this.newTodoName;
        this.$emit('update');
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