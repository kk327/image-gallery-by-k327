<script setup>
    const props = defineProps([
        "data"
    ])
</script>

<template>
    <button class="tileMain">
        <div class="background" :style="{ backgroundImage: 'url(' + data.url + ')' }"></div>
        <section class="mainTileContent">
            <img :style="{ content: 'url(' + data.url + ')' }">
            <p>{{ data.title }}</p>
        </section>
    </button>
</template>

<style scoped>
    img {
        height: 250px;
        width: 250px;
        object-fit: cover;
        border-radius: 20px;
        max-width: 90vw;
        background-color: rgb(0, 0, 0, 0.3);
    }

    .tileMain {
        width: 250px;
        height: 325px;
        color: white;
        max-width: 90vw;
        background-color: transparent;
        border: none;
        padding: 0;
        font-family: Arial, Helvetica, sans-serif;
        font-size: 100%;
        border-radius: 20px;
    }

    .background {
        filter: blur(100px);
        position: absolute;
        width: 275px;
        height: 325px;
        z-index: -1;
        background-size: cover;
    }

    p {
        font-weight: 600;
        height: 60px;
        position: relative;
        bottom: 15px;
        text-align: center;
        overflow-y: auto;
        overflow-x: hidden;
        text-wrap: wrap;
    }

    .mainTileContent {
        transition-duration: 200ms;
    }

    .mainTileContent:hover {
        scale: 90%;
        cursor: pointer;
    }
</style>