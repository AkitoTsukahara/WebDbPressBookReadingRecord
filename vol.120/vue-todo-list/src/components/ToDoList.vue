<template>
  <div>
    <input v-model="inputValue">
    <button v-on:click="handleClick">
      ADD
    </button>
    <input 
      v-model="filterValue"
      placeholder="フィルタテキスト">
    <ul>
      <li v-for="todo in filteredTodoItems" 
        v-bind:key="todo.id"
        v-bind:class="{'done': todo.done}"
        v-on:click="todo.done = !todo.done"
        class="todo-item"
        >
        <span v-if="todo.done">✔️</span>
        {{ todo.text}}
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
    const todoItems = [
        { id: 1, text: 'Go out to sea.', done: false},
        { id: 2, text: 'Invite the first member', done: true}
      ];
    return {
      inputValue: '',
      todoItems,
      filteredTodoItems: todoItems,
      filterValue: ''
    }
  },
  methods: {
    handleClick() {
      this.todoItems.push({
        id: this.todoItems.length + 1,
        text: this.inputValue
      })
      this.inputValue = ''
    },
    updateFilteredTodoItems() {
      this.filteredTodoItems = this.filterValue 
      ? this.todoItems.filter( (todo) => 
        todo.text.includes(this.filterValue)
      ): this.todoItems
    },
  },
  computed: {
    filteredTodoItems() {
      if (!this.filterValue) {
        return this.todoItems
      }
      return this.todoItems.filter((todo) => {
        return todo.text.includes(this.filterValue)
      })
    }
  },
  watch: {
    filterValue() {
      this.updateFilteredTodoItems()
    },
    todoItems: {
      handler() {
        this.updateFilteredTodoItems()
      },
      deep: true
    }
  }
}
</script>
<style>
.todo-item.done{
  background: #3fb983;
  color: #ffffff;
}
</style>