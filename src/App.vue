<template>
  <div class="todo-container">
    <div class="todo-wrap">
      <TodoHearder :addTodo="addTodo"/>
      <TodoList :todos="todos" :delTodo="delTodo"/>
      <TodoFooter :todos="todos" :deleteCheck="deleteCheck" :selectAll="selectAll"/>
    </div>
  </div>
</template>

<script>
  import TodoFooter from './components/TodoFooter'
  import TodoHearder from './components/TodoHeader'
  import TodoList from './components/TodoList'

  export default {
    name: 'App',
    components: {
      TodoList, TodoHearder, TodoFooter
    },
    data() {
      return {
        todos: [
          {title: 'eating', complete: false},
          {title: 'sleeping', complete: false}
        ]
      }
    },
    methods: {
      addTodo(todo) {
        this.todos.unshift(todo)
      },
      delTodo(index) {
        this.todos.splice(index, 1)
      },
      deleteCheck() {
        this.todos = this.todos.filter(todo => !todo.complete)
      },
      selectAll(isCheck) {
        this.todos.forEach(todo => todo.complete = isCheck)
      }
    }
  }
</script>

<style>
  .todo-container {
    width: 600px;
    margin: 0 auto;
  }

  .todo-container .todo-wrap {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
</style>
