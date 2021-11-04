<template>
    <div class="home">
        <n-card title="✔️ ToDo App" embedded>
            <template #default> 
                <n-input placeholder="ToDo" @change="addTodo">
                    <template #prefix>
                        <n-icon>
                            <add-circle-icon />
                        </n-icon>
                    </template>
                </n-input> 
                <n-scrollbar style="max-height: 300px">
                    <n-list bordered>
                        <n-list-item v-for="(todo, index) in todos" :key="index">
                            <n-thing>
                                <div class="li-content">
                                    <n-checkbox v-model:checked="todo.done" />
                                    <span :class="{'todo-done' : todo.done}">{{ todo.content }}</span>
                                    <n-button type="error" ghost @click="removeTodo(index)">
                                        <template #icon>
                                            <n-icon>
                                                <trash-icon />
                                            </n-icon>
                                        </template>
                                    </n-button>
                                </div>
                            </n-thing>
                        </n-list-item>
                        <h4 v-if="todos.length === 0">Empty list.</h4>
                    </n-list>
                </n-scrollbar>   
            </template>
        </n-card>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { AddCircleOutline as AddCircleIcon } from '@vicons/ionicons5'
import { Trash as TrashIcon } from '@vicons/ionicons5'

interface Todo {
  content: string,
  done: boolean
}

export default defineComponent({
  name: 'Home',
  components: {
      AddCircleIcon,
      TrashIcon
  },
  setup () {
    const defaultData = JSON.stringify([{
        done: false,
        content: 'Write a blog post'
    }]);

    const todosData = JSON.parse(localStorage.getItem('todos') || defaultData);
    const todos = ref(todosData);
    
    function addTodo (todoValue : string) {
        if (todoValue) {
            todos.value.push({
                done: false,
                content: todoValue
            });
            todoValue = '';
        }
        saveData();
    }
    function doneTodo (todo : Todo) {
        todo.done = !todo.done
        saveData();
    }
    function removeTodo (index : number) {
        todos.value.splice(index, 1);
        saveData();
    }
    function saveData () {
        const storageData = JSON.stringify(todos.value);
        localStorage.setItem('todos', storageData);
    }
    return {
        todos,
        addTodo,
        doneTodo,
        removeTodo,
        saveData
    }
  }
});
</script>

<style scoped>
.home {
    display: flex;
    justify-content: center;
    align-items: center;
    height: -webkit-fill-available;
}

.n-card {
  max-width: 300px;
}

.todo-done {
    text-decoration:line-through;
}

.li-content {
    display: flex;
    justify-content: space-between;
}
</style>