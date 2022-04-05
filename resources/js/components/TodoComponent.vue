<template>
    <div>
        <div
            class="vh-100 vw-100 d-flex flex-column justify-content-center align-items-center"
        >
            <div>
                <h1 class="text-center">Vuejs Todo App</h1>
                <form @submit.prevent="saveData">
                    <div class="input-group my-3">
                        <input
                            v-model="formData.title"
                            type="text"
                            class="form-control-lg"
                            placeholder="Type here"
                            aria-label="Recipient's username"
                            aria-describedby="button-addon2"
                        />
                        <div class="input-group-append">
                            <button
                                class="btn btn-outline-secondary btn-success mx-2"
                                type="submit"
                                id="button-addon2"
                            >
                                Add this to your list
                            </button>
                        </div>
                    </div>
                </form>
                <div v-if="todos.length !== 0" class="bg-light p-2 border rounded">
                    <div v-for="(todo, index) in todos" :key="todo.id">
                        <div
                            :class="index % 2 == 0 ? 'grey-todo' : 'white-todo'"
                            class="d-flex justify-content-between px-2 py-3"
                        >
                            <div>
                                <span class="pr-2">
                                    <i
                                        v-if="todo.completed"
                                        class="fas fa-check-circle"
                                        @click="toggleTodo(todo)"
                                    ></i>
                                    <i
                                        v-else
                                        class="far fa-check-circle"
                                        @click="toggleTodo(todo)"
                                    ></i> </span
                                >{{ todo.title }}
                            </div>
                            <div>
                                <i class="fas fa-edit px-3"></i
                                ><i
                                    class="fas fa-trash-alt"
                                    @click="deleteTodo(todo)"
                                ></i>
                            </div>
                        </div>
                        <hr />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    data() {
        return {
            formData: {
                title: "",
            },
            todos: [],
        };
    },
    methods: {
        // method that saves the new todo
        async saveData() {
            try {
                // called axios "post" type
                await axios.post("/api/todo", this.formData);
                this.formData.title = "";
                // executing the method to have all the todo
                this.getTodos();
            } catch (er) {
                alert("Error sending request");
                console.log(er.response.data);
            }
        },
        // method that returns all todo
        async getTodos() {
            // called axios "get" type
            await axios
                .get("/api/todo")
                .then((resp) => {
                    this.todos = resp.data;
                })
                .catch((error) => {
                    console.log(error);
                });
        },
        // method that manages "completed" value
        async toggleTodo(todo) {
            todo.completed = !todo.completed;
            if (todo.completed) {
                try {
                    // called axios "put" type
                    await axios.put("/api/todo/" + todo.id, {
                        completed: 1,
                    });
                } catch (er) {
                    alert("Error sending request");
                    console.log(er.response.data);
                }
            } else {
                try {
                    // called axios "put" type
                    await axios.put("/api/todo/" + todo.id, {
                        completed: 0,
                    });
                } catch (er) {
                    alert("Error sending request");
                    console.log(er.response.data);
                }
            }
        },
        // method that eliminates a todo
        async deleteTodo(todo) {
            try {
                // called axios "delete" type
                await axios.delete("/api/todo/" + todo.id);
                // executing the method to have all the todo
                this.getTodos();
            } catch (er) {
                alert("Error sending request");
                console.log(er.response.data);
            }
        },
    },
    mounted() {
        // executing the method to have all the todo
        this.getTodos();
    },
};
</script>
