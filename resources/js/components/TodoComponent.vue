<template>
    <div>
        <div
            class="vh-100 vw-100 d-flex flex-column justify-content-center align-items-center"
        >
            <h1>Vuejs Todo App</h1>
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
        };
    },
    methods: {
        async saveData() {
            try {
                const resp = await axios.post("/api/todo", this.formData);
                console.log(resp);
            } catch (er) {
                alert("Errore nell'invio della richiesta");
                console.log(er.response.data);
            }
        },
    },

    mounted() {},
};
</script>
