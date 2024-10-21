<script setup>
    const emit = defineEmits([
        "changePage"
    ]);

    const props = defineProps([
        "page",
        "maxPages",
        "disable"
    ]);

    function changePage(value) {
        emit("changePage", props.page + value);
        window.scrollTo(0, 0);
    }
</script>

<template>
    <footer>
        <button @click="page == 1 ? {} : changePage(-1)" :class="{ grayedButton: page == 1 }" :disabled="disable" title="Previous page">
            <img src="@/assets/arrow.png" alt="<" id="leftButtonImage">
        </button>
        <p>Page {{ maxPages > 0 ? page : "0" }} / {{ maxPages }}</p>
        <button @click="page >= maxPages ? {} : changePage(1)" :class="{ grayedButton: page >= props.maxPages }" :disabled="disable" title="Next page">
            <img src="@/assets/arrow.png" alt=">">
        </button>
    </footer>
</template>

<style scoped>
    footer {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 60px;
        background-color: rgb(0, 0, 0, 0.3);
        z-index: 99;
        color: white;
        font-weight: 600;
        width: 100vw;
        bottom: 0;
    }

    p {
        margin: 0 20px;
    }

    button {
        height: 35px;
        width: 35px;
        border: none;
        border-radius: 20px;
        transition-duration: 200ms;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: white;
    }

    button:hover:not([class="grayedButton"]) {
        cursor: pointer;
        scale: 112.5%;
    }

    .grayedButton {
        background-color: gray !important;
    }

    img {
        height: 17.5px;
    }

    #leftButtonImage {
        rotate: 180deg;
    }
</style>