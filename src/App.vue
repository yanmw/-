<template>
  <div class="todo-container">
    <div class="todo-wrap">
      <TodoHearder @addTodo="addTodo"/><!--自定义事件，此种传递只适用于父子组件，超过两级无法使用。-->
      <TodoList :todos="todos" :delTodo="delTodo"/>
      <!--<TodoFooter :todos="todos" :deleteCheck="deleteCheck" :selectAll="selectAll"/>-->
      <TodoFooter>
        <input type="checkbox" v-model="isAllCheck" slot="checkAll"/>
        <span slot="count">已完成{{completeSize}} / 全部{{todos.length}}</span>
        <button class="btn btn-danger" v-show="completeSize>0" @click="deleteCheck()" slot="delete">清除已完成任务</button>
      </TodoFooter>
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
    computed: {
      completeSize() {
        //array.reduce(function(total, currentValue, currentIndex, arr), initialValue).
        //function(total,currentValue, index,arr)	必需。用于执行每个数组元素的函数。
        //函数参数:
        //total	必需。初始值, 或者计算结束后的返回值。
        //currentValue	必需。当前元素
        //currentIndex	可选。当前元素的索引
        //arr	可选。当前元素所属的数组对象。
        //initialValue	可选。传递给函数的初始值
        return this.todos.reduce((preTotal, todo) => preTotal + (todo.complete ? 1 : 0), 0)
      },
      isAllCheck: {
        get() {
          return this.completeSize === this.todos.length && this.completeSize > 0;
        },
        set(value) {
          this.selectAll(value)
        }
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
