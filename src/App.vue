<template>
  <div class="todo-container">
    <div class="todo-wrap">
      <Header :addTodo="addTodo" />
      <List :todos="todos" :deleteTodo="deleteTodo" :updateTodo="updateTodo" />
      <Footer
        :todos="todos"
        :checkAll="checkAll"
        :clearAllCompletedTodos="clearAllCompletedTodos"
      />
    </div>
  </div>
</template>

<script lang='ts'>
import { defineComponent, onMounted, reactive, toRefs, watch } from 'vue'

import Header from './components/Header.vue'
import List from './components/List.vue'
import Footer from './components/Footer.vue'

import { Todo } from './types/todo'

import { saveTodos, readTodos } from './utils/localstorageUtils'

export default defineComponent({
  name: 'App',
  components: {
    Header,
    List,
    Footer
  },

  setup() {
    const state = reactive<{ todos: Todo[] }>({
      todos: []
    })

    /* 添加todo */
    const addTodo = (todo: Todo) => {
      state.todos.unshift(todo)
    }
    /* 删除todo */
    const deleteTodo = (index: number) => {
      state.todos.splice(index, 1)
    }

    /* 更新todo */
    const updateTodo = (todo: Todo, isCompleted: boolean) => {
      todo.isCompleted = isCompleted
    }

    /* 全选/全不选 */
    const checkAll = (isCompleted: boolean) => {
      state.todos.forEach((todo) => {
        todo.isCompleted = isCompleted
      })
    }

    /* 清除所有选中的todo */
    const clearAllCompletedTodos = () => {
      state.todos = state.todos.filter((todo) => !todo.isCompleted)
    }

    /* 组件挂载后 */
    onMounted(() => {
      setTimeout(() => {
        state.todos = readTodos()
      }, 1000)
    })

    watch(() => state.todos, saveTodos, { deep: true })

    return {
      ...toRefs(state),
      addTodo,
      deleteTodo,
      updateTodo,
      checkAll,
      clearAllCompletedTodos
    }
  }
})
</script>

<style>
/*app*/
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