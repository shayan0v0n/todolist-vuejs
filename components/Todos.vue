<script>
import axios from 'axios'
export default {
    props: {
        todoTitle: String,
        todoDesc: String,
        todos: String
    },
    emits: {
        "deleteTodoItem": function() {}
    },
    methods: {
        deleteTodoHandler(keyitem) {
            axios.delete(`https://todo-list-104ff-default-rtdb.firebaseio.com/todos/${keyitem}.json`)
            console.log(keyitem)
            this.$emit("deleteTodoItem", keyitem);
        }
    }
}
</script>

<template>
    <div class="todo-item animate__animated animate__fadeIn" :title="todoDesc">
        <span class="todo-item__item">{{todoTitle}} </span>
        <button class="todo-item__button" @click="deleteTodoHandler(todos)">Delete</button>
    </div>
</template>

<style>
    .todo-item {
        padding: .5rem;
        margin: .7rem;
        box-shadow: rgba(0, 0, 0, 0.12) 0px 1px 3px, rgba(0, 0, 0, 0.24) 0px 1px 2px;
        display: flex;
        justify-content: space-around;
        align-items: center;
        border-radius: 5px;
    }

    .todo-item__item {
        font-size: 1.4rem;
        color:#1E5128;
        font-weight: bold;
    }

    .todo-item__button {
        border: none;
        outline: none;
        background: #9f3d3d;
        border-radius: 5px;
        color: white;
        font: inherit;
        box-shadow: rgba(0, 0, 0, 0.4) 0px 2px 4px, rgba(0, 0, 0, 0.3) 0px 7px 13px -3px, rgba(0, 0, 0, 0.2) 0px -3px 0px inset;
        padding: .5rem .7rem;
        cursor: pointer;
        transition: all .3s;
    }

    .todo-item__button:hover {
        background: #511e1e;
    }
</style>