<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addItemEvent="addOneItem"></TodoInput>
    <TodoList v-bind:propsdata="todoItems" v-on:removeItemEvent="removeOneItem" 
     v-on:toggleItemEvent="toggleOneItem"
    ></TodoList>
    <TodoFooter v-on:removeAllItemEvent="removeAllItems"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  data: function() {
        return {
            todoItems: []
        }
  },
  methods: {
    addOneItem: function(todoItem) {
      var obj = {completed: false, item: todoItem};
      localStorage.setItem(todoItem,JSON.stringify(obj));
      this.todoItems.push(obj);
    },
    removeOneItem: function(todoItem, index) {
      localStorage.removeItem(todoItem.item);
      this.todoItems.splice(index, 1);
    },
    toggleOneItem: function(todoItem, index) {
      //todoItem.completed = !todoItem.completed;
      this.todoItems[index].completed = !this.todoItems[index].completed;
      //localStorage에 updateItem 메서드가 없어서 removeItem하고 setItem 한다.
      localStorage.removeItem(todoItem.item);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    },
    removeAllItems: function() {
      localStorage.clear();
      this.todoItems = [];
    }
  },
  /* life cycle method */
  created: function() {
      if(localStorage.length > 0){
          for(var i=0; i < localStorage.length; i++) {
              if(localStorage.key(i) !== 'loglevel:webpack-dev-server'){
                  this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
              }
          }
      }
  },
  components: {
    'TodoHeader': TodoHeader,
    'TodoInput': TodoInput,
    'TodoList': TodoList,
    'TodoFooter': TodoFooter
  }
}
</script>

<style>
  body {
    text-align: center;
    background-color: #f6f6f6;
  }
  input {
    border-style: groove;
    width:200px;
  }
  button {
    border-style: groove;
  }
  .shadow {
    box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
  }
</style>
