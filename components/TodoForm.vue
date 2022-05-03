<script>
import axios from 'axios';
export default {
    data() {
        return {
            title: "",
            description: "",
            todos: null
        }
    },
    emits: {
        "rerenderTodos": function() {}
    },
    methods: {
        setTitleData(event) {
            this.title = event.target.value
        },
        setDescData(event) {
            this.description = event.target.value
        },
        submitHandler(event) {
            event.preventDefault()
            const newData = {
                title: this.title,
                description: this.description
            }
            axios.post("https://todo-list-104ff-default-rtdb.firebaseio.com/todos.json", newData)
            this.clearProducts(newData)
            this.title = ""
            this.description = ""
        },
        clearProducts(newData) {
            this.$emit("rerenderTodos", newData);
        }
    }
}

</script>

<template>
    <form class="form-style">
        <input
         type="text"
         placeholder="Title..."
         class="form-style__imputs"
         @input="setTitleData($event)"
         :value="title"
         required />
        <input
         type="text"
         placeholder="Description..."
         class="form-style__imputs"
         @input="setDescData($event)"
         :value="description"
         required />
        <button
         class="form-style__button"
         @click="submitHandler($event)">Submit</button>
    </form>
</template>

<style>
    .form-style {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-flow: column nowrap;
    }
    .form-style__imputs {
        width: 50%;
        margin: .5rem;
        padding: .5rem;
        background: transparent;
        outline: none;
        border-radius: 5px;
        border: 1px solid #4E9F3D;
        color: #191A19;
        transition: all .2s;
    }
    .form-style__imputs:focus {
        border: 2px solid #1E5128;
    }

    .form-style__button {
        border: none;
        outline: none;
        background: #4E9F3D;
        border-radius: 5px;
        width: 30%;
        color: white;
        font: inherit;
        box-shadow: rgba(0, 0, 0, 0.4) 0px 2px 4px, rgba(0, 0, 0, 0.3) 0px 7px 13px -3px, rgba(0, 0, 0, 0.2) 0px -3px 0px inset;
        padding: .5rem .7rem;
        cursor: pointer;
        transition: all .3s;
    }
    .form-style__button:hover {
        background: #1E5128;
    }
</style>