<template>
    <div id="app">
    <AppHeader/>
      <AppFilters :active-filters="activeFilters" @shift-filter="shiftFilter"/>
      <main class="app-main">
        <AppTodoList :todos="filteredTodos" @toggle-todo="toggleTodo" @remove-todo="removeTodo"/>
        <AppAddTodo @add-todo="addTodo"/>
      </main>
      <AppFooter/>
    </div>
</template>

<script lang="ts">
  import {defineComponent} from 'vue';
  import AppHeader from "./components/AppHeader.vue"
  import AppFilters from "./components/AppFilters.vue"
  import AppTodoList from "./components/AppTodoList.vue"
  import AppAddTodo from "./components/AppAddTodo.vue"
  import AppFooter from "./components/AppFooter.vue"
  import {Todo} from "@/types/Todo";
  
  interface State {
    todos: Todo[],
    activeFilters: string
  }
  
  export default defineComponent({
    components: {
      AppHeader,
      AppFilters,
      AppTodoList,
      AppAddTodo,
      AppFooter,
    }, 
    data():State{
      return{
        todos:[],
        activeFilters: 'All'
      }
    },
    computed: {
      filteredTodos() :Todo[]{
        switch(this.activeFilters){
          case 'Active':
            return this.todos.filter((todo)=>!todo.completed)
          case 'Done':
            return this.todos.filter((todo)=>todo.completed)
          case 'All':
          default:
            return this.todos
        }
      }
    },
    methods:{
      addTodo(todo:Todo){
        this.todos.push(todo)
        this.setTodosToStorage()
      },
      toggleTodo(id:number){
        const targetTodo = this.todos.find((todo:Todo)=>{
          return todo.id === id
        })

        if(targetTodo){
          targetTodo.completed = !targetTodo.completed
        }
        this.setTodosToStorage()
      },
      removeTodo(id:number){
        this.todos = this.todos.filter((todo)=>{
          return todo.id != id
        })
        this.setTodosToStorage()
      },
      shiftFilter(filter:any){
        this.activeFilters = filter
      },
      setTodosToStorage(){
        localStorage.setItem('todos', JSON.stringify(this.todos))
      },
      getTodosFromStorage(){
        console.log('mounted')
        const  storageTodos = localStorage.getItem('todos');
        if(storageTodos){
          console.log(JSON.parse(storageTodos))
          this.todos = JSON.parse(storageTodos)
        } else{
          this.todos = []
        }
      }
    },
    mounted(){
      this.getTodosFromStorage()
    }
  })
</script>