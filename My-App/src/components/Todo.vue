<template>
  <div id="container">
    <AddTodo @add-todo="addTodo"/>
    <TodoItem
      v-for="todo in todos"
      :key="todo.id"
      :todoProps="todo"
      @item-completed="markComplete"
      @delete-item="deleteTodo"
    />
  </div>
</template>

<script>
import { ref } from 'vue'
// import { v4 as uuidv4 } from 'uuid'
import TodoItem from './TodoItem'
import AddTodo from './AddTodo'
import axios from 'axios'
export default {
  name: 'Todo',
  components: { TodoItem, AddTodo },

  setup() {
    const todos = ref([])
    const getAllTodos = async () =>{
      try{
        const res = await axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
        todos.value = res.data
        console.log(res.data)
      }catch(error){
        console.log(error)
      }
    }
    getAllTodos()
    const markComplete = id => {
      // console.log(id);
      todos.value = todos.value.map(todo => {
        if (todo.id === id) todo.completed = !todo.completed
        return todo
      })
    }
    const deleteTodo = async id => {
      try {
        await axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        todos.value = todos.value.filter(todo => todo.id !== id)
      } catch (error) {
        console.log(error)
      }
      // todos.value = todos.value.filter(todo => todo.id !== id)
    }
    const addTodo = async newTodo =>{
      // todos.value.push(newTodo)
      try {
        const res = await axios.post('https://jsonplaceholder.typicode.com/todos', newTodo)
        todos.value.push(res.data)
      } catch (error) {
        console.log(error)
      }

    }
    return {
      todos,
      markComplete,
      deleteTodo, 
      addTodo,
      // getAllTodos
    }
  }
}
</script>
  
<style scoped>
#container {
  width: 70%;
  margin: auto;
}
</style>