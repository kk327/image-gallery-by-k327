<script setup>
    const props = defineProps([
        "siteInformation"
    ])
</script>

<template>
    <section id="popupPanel">
        <p v-html="siteInformation"></p>
        <button @click="$emit('hideSiteInformation')" title="Close">
            <img src="@/assets/close.png" alt="X">
        </button>
    </section>
</template>

<style scoped>
    #popupPanel {
        position: fixed;
        width: 100vw;
        height: 100vh;
        background-image: linear-gradient(rgb(0, 0, 0, 0.9), rgb(0, 0, 0, 0.95));
        z-index: 100;
        display: flex;
        justify-content: center;
        align-items: center;
        color: white;
        text-align: center;
    }

    button {
        height: 40px;
        width: 40px;
        border: none;
        border-radius: 20px;
        transition-duration: 200ms;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: rgb(255, 170, 170);
        position: absolute;
        top: 10px;
        right: 10px;
    }

    button:hover {
        cursor: pointer;
        scale: 112.5%;
    }

    img {
        height: 20px;
    }

    p {
        max-height: calc(100vh - 120px);
        max-width: 900px;
        text-wrap: wrap;
        overflow-y: auto;
        padding: 5px;
        box-sizing: border-box;
        display: inline;
    }
</style>