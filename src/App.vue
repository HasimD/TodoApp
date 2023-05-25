<template>
    <main class="app">
        <section>
            <h2>SAMPLE TODO APP</h2>
        </section>

        <h5>Create a new todo item:</h5>

        <section class="create-todo">
            <form @submit.prevent="addTodo">
                <input type="text" placeholder="e.g. be born" 
                v-model="newItemText">

                <input type="submit" value="Add">
            </form>
        </section>

        <TodoList></TodoList>
    </main>
</template>

<script setup lang="ts">
import {ref, onMounted, watch, provide} from 'vue'
import {TodoItem} from './todo-item'
import TodoList from './TodoList.vue';

const todoList = ref<TodoItem[]>([])

const newItemText = ref<string>('')

const addTodo = () => {
    if(newItemText.value.trim() === '') return
    
    todoList.value.push({
        content: newItemText.value,
        done: false,
        editable:false,
        createdAt: new Date().getTime()
    })
    newItemText.value = ''
}

watch(todoList, (newValue: TodoItem[]) => {
    localStorage.setItem('todoList', JSON.stringify(newValue))
}, {deep: true})

onMounted(() => {
    todoList.value = JSON.parse(localStorage.getItem('todoList') || '')
})

provide('todoList', todoList)
</script>