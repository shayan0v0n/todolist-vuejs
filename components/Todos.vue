<script>
import axios from 'axios'
export default {
    data() {
        return {
            updateExist: false,
            titleUpdate: '',
            descUpdate: '',
            todos: []
        }
    },
    props: {
        todoTitle: String,
        todoDesc: String,
        todos: String
    },
    emits: {
        "deleteTodoItem": function() {},
        "updateTodoList": function() {}
    },
    methods: {
        deleteTodoHandler(keyitem) {
            axios.delete(`https://todo-list-104ff-default-rtdb.firebaseio.com/todos/${keyitem}.json`)
            console.log(keyitem)
            this.$emit("deleteTodoItem", keyitem);
        },
        updateTodoHandler(todoTitle, todoDesc) {
            this.updateExist = true
            this.titleUpdate = todoTitle;
            this.descUpdate = todoTitle;
        },
        updateTitleHandler(event) {
            this.titleUpdate = event.target.value
            console.log(this.titleUpdate)
        },
        updatedHandler(keyitem) {
            axios.patch(`https://todo-list-104ff-default-rtdb.firebaseio.com/todos/${keyitem}.json`, {
                title: this.titleUpdate,
                description: this.descUpdate
            })
            this.updateExist = false;
            this.$emit("updateTodoList")
        }
    }
}
</script>

<template>
    <div class="todo-item animate__animated animate__fadeIn" :title="todoDesc">
        <div v-if="!updateExist">
            <span class="todo-item__item"> {{todoTitle}} </span>
        </div>
        <div v-else>
            <input :value="todoTitle" @input="updateTitleHandler($event)" />
        </div>
        <div v-if="!updateExist">
            <button class="todo-item__delete" @click="deleteTodoHandler(todos)">Delete</button>
            <button class="todo-item__update" @click="updateTodoHandler(todoTitle, todoDesc)">Update</button>
        </div>
        <div v-else>
            <button class="todo-item__update" @click="updatedHandler(todos)">Edit Submit</button>
        </div>
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

    .todo-item__delete {
        margin: .5rem;
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

    .todo-item__delete:hover {
        background: #511e1e;
    }

    .todo-item__update {
        margin: .5rem;
        border: none;
        outline: none;
        background: #3d9f45;
        border-radius: 5px;
        color: white;
        font: inherit;
        box-shadow: rgba(0, 0, 0, 0.4) 0px 2px 4px, rgba(0, 0, 0, 0.3) 0px 7px 13px -3px, rgba(0, 0, 0, 0.2) 0px -3px 0px inset;
        padding: .5rem .7rem;
        cursor: pointer;
        transition: all .3s;
    }

    .todo-item__update:hover {
        background: #1e5122;
    }
</style>