<template>
    <div>
        <TodoItem 
        v-bind:key="todo.id" 
        v-for="todo in list" 
        v-bind:todo="todo"
        v-on:del-todo="$emit('del-todo', todo.id)"
         />
    </div>
</template>

<script>
import TodoItem from './TodoItem.vue'

export default {
    name: "Todos_page",
    components: {
        TodoItem
    },
    props: ["list"],
}
</script>

<style scoped>

</style>
