<template>
  <div class="todo-footer">
    <label>
      <input type="checkbox" v-model="isAllCheck"/>
    </label>
    <span>
          <span>已完成{{completeSize}}</span> / 全部{{todos.length}}
        </span>
    <button class="btn btn-danger" v-show="completeSize>0" @click="deleteCheck()">清除已完成任务</button>
  </div>
</template>

<script>
  export default {
    name: "TodoFooter",
    props: {
      todos: Array,
      deleteCheck: Function,
      selectAll: Function
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
    }
  }
</script>

<style scoped>
  .todo-footer {
    height: 40px;
    line-height: 40px;
    padding-left: 6px;
    margin-top: 5px;
  }

  .todo-footer label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
  }

  .todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 5px;
  }

  .todo-footer button {
    float: right;
    margin-top: 5px;
  }
</style>
