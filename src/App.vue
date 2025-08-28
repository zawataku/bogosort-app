<template>
    <div id="app">
        <BogoSortVisualizer />
    </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import BogoSortVisualizer from "./components/Bogo.vue";

export default defineComponent({
    name: "App",
    components: {
        BogoSortVisualizer,
    },
});
</script>
