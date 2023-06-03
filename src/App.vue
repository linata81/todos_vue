<!-- eslint-disable vue/return-in-computed-property -->
<template>
  <AppHeader/>

  <AppFilters :active-filter="activeFilter" @set-filter="setFilter"/>

  <main class="app-main">
    <AppTodoList :todos="filterdTodos" @toggle-todo="toggleTodo" @remove-todo="removeTodo"/>

    <AppAddTodo @add-todo="addTodo"/>
  </main>

  <AppFooter :stats="stats"/>
</template>

<script lang="ts">
import {defineComponent} from 'vue';
import { Todo } from '@/types/Todo';
import AppHeader from './components/AppHeader.vue';
import AppFilters from './components/AppFilters.vue';
import AppTodoList from './components/AppTodoList.vue';
import AppAddTodo from './components/AppAddTodo.vue';
import AppFooter, { Stats } from './components/AppFooter.vue';
import { Filter } from './types/Filter';

interface State {
  todos: Todo[],
  activeFilter: Filter
}

export default defineComponent({
  components: {
    AppHeader,
    AppFilters,
    AppTodoList,
    AppAddTodo,
    AppFooter
  },
  data():State {
    return {
      todos: [
        { id: 0, text: 'Learn the basics of Vue', completed: true },
        { id: 1, text: 'Learn the basics of Typescript', completed: false },
        { id: 2, text: 'Subscribe to the channel', completed: false },
      ],
      activeFilter: 'All'
    }
  }, 
  computed: {
    filterdTodos(): Todo[] {
      switch(this.activeFilter) {
        case 'Active':
          // return this.todos.filter(todo => !todo.completed) дублирование кода убираем строчкой ниже
          return this.activeTodos
        case 'Done':
          // return this.todos.filter(todo => todo.completed)
          return this.doneTodos
        case 'All':
        default:
          return this.todos
      }
    },
    stats(): Stats {
      return {
        // active: this.todos.filter(todo => !todo.completed).length, дублирование кода убираем строчкой ниже
        active: this.activeTodos.length,
        // done:this.todos.filter(todo => todo.completed).length
        done:this.doneTodos.length
      }
    },
    activeTodos(): Todo[] {
      return this.todos.filter(todo => !todo.completed)
    },
    doneTodos(): Todo[] {
      return this.todos.filter(todo => todo.completed)
    },
  },
  methods: {
    addTodo(todo: Todo) {
      // console.log(todo); 
      this.todos.push(todo);   
    },
    toggleTodo(id:number) {
      // console.log(id);
      const targetTodo = this.todos.find((todo: Todo) => todo.id === id);
      
      if(targetTodo) {
        targetTodo.completed = !targetTodo.completed;
      }
      
    },
    removeTodo(id:number) {
      this.todos = this.todos.filter((todo:Todo) => todo.id !== id);
    },
    setFilter(filter: Filter){
      this.activeFilter = filter;
    }
  }
});
</script>