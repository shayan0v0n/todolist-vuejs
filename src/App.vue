<script>
import axios from 'axios'
    import TodoForm from '../components/TodoForm.vue'
    import Todos from '../components/Todos.vue'
    import Loading from '../components/Loading.vue'
    import Error from '../components/Error.vue'
    export default {
        data() {
            return {
                todos: null,
                error: false
            }
        },
        components: {
            TodoForm,
            Todos,
            Loading,
            Error
        },
        mounted () {
            axios.get("https://todo-list-104ff-default-rtdb.firebaseio.com/todos.json")
                .then(res => res)
                    .then(res => this.todos = res.data)
                        .catch(err => this.error = true)

        },
        methods: {
            rerenderTodoHandler(item) { 
                this.todos = {
                    ...this.todos,
                    item
                }
            },
            deleteTodoItemHandler(item) {
                delete this.todos[item]
            }
        }
    }
</script>

<template>
    <div class="main-app animate__animated animate__fadeInDown">
        <TodoForm @rerenderTodos="rerenderTodoHandler"/>
    </div>
    <div class="main-app animate__animated animate__fadeInUp">
        <div v-if="todos" v-for="(todo, index) in todos">
            <Todos :todoTitle="todo.title" :todoDesc="todo.description" :todos="index" @deleteTodoItem="deleteTodoItemHandler" />
        </div>
        <div v-else-if="error">
            <Error />
        </div>
        <div v-else>
            <Loading />
        </div>
    </div>
</template>

<style>
    .main-app {
        width: 70%;
        margin: 1rem auto;
        padding: 20px;
        background: white;
        border-radius: 5px;
        box-shadow: rgba(0, 0, 0, 0.15) 1.95px 1.95px 2.6px;
    }
</style>