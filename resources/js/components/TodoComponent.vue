<template>
    <div>
        <div
            class="vh-100 vw-100 d-flex flex-column justify-content-center align-items-center"
        >
            <div class="container">
                <h1 class="text-center">Full Stack Todo App</h1>
                <form @submit.prevent="saveData">
                    <div class="input-group my-3 d-flex justify-content-center">
                        <div>
                            <input
                                v-model="formData.title"
                                type="text"
                                class="form-control-lg"
                                placeholder="Type here"
                                aria-label="Recipient's username"
                                aria-describedby="button-addon2"
                            />
                        </div>

                        <div class="input-group-append">
                            <button
                                class="btn btn-success mx-2"
                                type="submit"
                                id="button-addon2"
                            >
                                Add this to your list
                            </button>
                        </div>
                    </div>
                </form>
                <div
                    v-if="todos.length !== 0"
                    class="bg-light p-2 border rounded"
                >
                    <div v-for="(todo, index) in todos" :key="todo.id">
                        <div
                            :class="
                                todo.completed ? 'green-todo' : 'white-todo'
                            "
                            class="d-flex justify-content-between align-items-center px-3 todo-box"
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
                                        class="far fa-circle"
                                        @click="toggleTodo(todo)"
                                    ></i>
                                </span>
                                <span
                                    v-if="editMode != index"
                                    class="todo-title"
                                    >{{ todo.title }}</span
                                >
                                <input
                                    v-if="editMode === index"
                                    v-model="todo.title"
                                    type="text"
                                    @keyup.enter="updateTodo(todo)"
                                />
                            </div>
                            <div>
                                <i
                                    class="fas fa-edit px-3"
                                    @click="editMethod(index)"
                                    v-if="editMode != index"
                                ></i>
                                <i
                                    class="fas fa-edit px-3 edit-mode"
                                    @click="updateTodo(todo)"
                                    v-else
                                ></i>
                                <i
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
            editMode: null,
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
                    // called axios "patch" type
                    await axios.patch("/api/todo/" + todo.id, {
                        completed: 1,
                    });
                } catch (er) {
                    alert("Error sending request");
                    console.log(er.response.data);
                }
            } else {
                try {
                    // called axios "patch" type
                    await axios.patch("/api/todo/" + todo.id, {
                        completed: 0,
                    });
                } catch (er) {
                    alert("Error sending request");
                    console.log(er.response.data);
                }
            }
        },
        // method that update a todo title
        async updateTodo(todo) {
            this.editMode = null;
            try {
                // called axios "patch" type
                await axios.patch("/api/todo/" + todo.id, {
                    title: todo.title,
                });
            } catch (er) {
                alert("Error sending request");
                console.log(er.response.data);
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
        editMethod(x) {
            this.editMode = x;
        },
    },
    mounted() {
        // executing the method to have all the todo
        this.getTodos();
    },
};
</script>

<style lang="scss" scoped></style>
