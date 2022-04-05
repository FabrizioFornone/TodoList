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
                <div class="bg-light p-2 border rounded">
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
                                ><i class="fas fa-trash-alt"></i>
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
            todos: "",
        };
    },
    methods: {
        async saveData() {
            try {
                const resp = await axios.post("/api/todo", this.formData);
                this.formData.title = "";
                this.getTodos();
            } catch (er) {
                alert("Errore nell'invio della richiesta");
                console.log(er.response.data);
            }
        },
        async getTodos() {
            await axios
                .get("/api/todo")
                .then((resp) => {
                    this.todos = resp.data;
                })
                .catch((error) => {
                    console.log(error);
                });
        },
        toggleTodo(todo) {
            todo.completed = !todo.completed;
            if (todo.completed) {
                axios.put("/api/todo/" + todo.id, {
                    completed: 1,
                });
            } else {
                axios.put("/api/todo/" + todo.id, {
                    completed: 0,
                });
            }
        },
    },
    mounted() {
        this.getTodos();
    },
};
</script>
