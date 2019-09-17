<template>
  <div class="todo-container">
    <div class="todo-wrap">
      <TodoHearder @addTodo="addTodo"/><!--自定义事件，此种传递只适用于父子组件，超过两级无法使用。-->
      <TodoList :todos="todos" :delTodo="delTodo"/>
      <TodoFooter :todos="todos" :deleteCheck="deleteCheck" :selectAll="selectAll"/>
    </div>
  </div>
</template>

<script>
  import PubSub from 'pubsub-js'
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
        todos: JSON.parse(window.localStorage.getItem('todos_key') || '[]')
      }
    },
    mounted(){  //消息的订阅和发布，比组件之间传递方法方便
      PubSub.subscribe('updateTodo',(msg,index)=>{
        this.updateTodo(index)
      })
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
      },
      updateTodo(index){
        const todo = {title:"aaa",complete:false}
        this.todos.splice(index,1,todo)
      }
    },
    watch:{
      todos:{
        deep:true,
        handler:function (value) {
          window.localStorage.setItem('todos_key',JSON.stringify(value))
        }
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
