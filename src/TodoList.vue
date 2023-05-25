<template>
    <section class="todo-list">
        <h3>TO DO LOST</h3>
        <div class="list">
            <div v-for="todo in todoListAscendingOrder" :key="todo.createdAt" :class="`todo-item ${todo.done && 'done'}`">
                <label>
                    <input type="checkbox" v-model="todo.done" />
                    <span :class="`bubble`"></span>
                </label>

                <div class="todo-content">
                    <input type="text" v-model="todo.content" />
                </div>

                <button class="delete" @click="removeTodo(todo)">Delete</button>
            </div>
        </div>
    </section>
</template>

<script lang="ts">
export default {
  name: 'TodoList',
};
</script>

<script setup lang="ts">
import {computed, inject, Ref} from 'vue'
import {TodoItem} from './todo-item'

const todoList: Ref<TodoItem[]> = inject('todoList')!;

const removeTodo = (removeItem: TodoItem) => {
    todoList.value = todoList.value.filter(item => item !== removeItem)
}

const todoListAscendingOrder = computed(() => {
  const sortedList = [...todoList.value].sort((x, y) => {
    return x.createdAt - y.createdAt;
  });

  const todoDoneList = sortedList.filter(item => item.done);
  const todoNotDoneList = sortedList.filter(item => !item.done);

  return [...todoNotDoneList, ...todoDoneList];
});
</script>
