<template>
    <div class="position-relative">
        <todo-component />
        <footer-component />
    </div>
</template>

<script>
import TodoComponent from "../components/TodoComponent.vue";

import FooterComponent from "../components/FooterComponent.vue";

export default {
    components: { TodoComponent, FooterComponent },
};
</script>

<style lang="scss" scoped></style>
