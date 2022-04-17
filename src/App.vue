<template>
  <div id="app">
    <div class="todo-container">
      <div class="todo-wrap">      
        <my-header :addTodo="addTodo"></my-header>
        <list :todos="todos" :checkTodo="checkTodo" :deleteTodo="deleteTodo"></list>      
        <my-footer :todos="todos" :checkAllTodo="checkAllTodo" :clearAllTodo="clearAllTodo"></my-footer>
      </div>
    </div>
  </div>
</template>

<script>

import List from './components/List.vue'
import MyFooter from './components/MyFooter.vue'
import MyHeader from './components/MyHeader.vue'

export default {
  components: { MyHeader, List, MyFooter },
  name: 'App',
  data() {
    return {
      todos: JSON.parse(localStorage.getItem('todos')) || [],
    }
  },
  watch: {
    todos:{
      deep:true,
      handler(value){
        localStorage.setItem('todos',JSON.stringify(value))
      }
    }
  },
  methods: {
    // 子传父。需要在父组件中定义一个函数，将函数传给子组件，子组件调用这个函数。可以将数据传给父组件。
    addTodo(totoObj) {
      console.log('我是app组件,我收到了数据:',totoObj)
      this.todos.unshift(totoObj)
    },
    // 勾选或者取消勾选一个todo
    checkTodo(id) {
      this.todos.forEach((todo)=>{
      if(todo.id === id) todo.done = !todo.done
    })
    },
    // 删除一个todo
    deleteTodo(id) {
      this.todos = this.todos.filter((todo)=>{
        return todo.id !== id
      })
    },
    // 全选or取消全选
    checkAllTodo(done) {
      this.todos.forEach((todo)=>{
        todo.done = done
      })
    },
    // 清楚所有已经完成的todo
    clearAllTodo(){
      this.todos = this.todos.filter((todo)=>{
        return !todo.done
      })
    }
  }
  

}

</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-edit {
  color: #fff;
  background-color: lightgreen;
  border: 1px solid green;
  margin-right: 5px;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn-edit:hover {
  color: #fff;
  background-color: green;
}

.btn:focus {
  outline: none;
}

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
