<script setup>
    const props = defineProps([
        "filterByFavorites",
        "siteInformation",
        "allowFavoriting"
    ])
</script>

<template>
    <header>
        <button id="infoButton" v-if="siteInformation" @click="$emit('showSiteInformation')" title="Site information">
            <img src="@/assets/info.png" alt="i">
        </button>
        <input type="text" placeholder="🔎 Search" @input="(e) => $emit('updateSearch', e.target.value)">
        <button id="filterByFavoritesButton" v-if="allowFavoriting" @click="$emit('filterByFavorites')" :style="{ backgroundColor: filterByFavorites ? 'rgb(255, 200, 0)' : 'rgb(255, 237, 170)' }" title="Filter by favorited">
            <img src="@/assets/star.png" alt="⭐">
        </button>
    </header>
</template>

<style scoped>
    header {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 60px;
        background-color: rgb(0, 0, 0, 0.3);
        z-index: 99;
    }

    input {
        height: 40px;
        width: 400px;
        max-width: calc(90vw - 100px);
        border: none;
        border-radius: 20px;
        text-align: center;
        font-size: 110%;
        font-weight: 600;
        outline: none;
        transition-duration: 200ms;
    }

    input:hover {
        scale: 103.5%;
    }

    button {
        height: 40px;
        min-width: 40px;
        border: none;
        border-radius: 20px;
        transition-duration: 200ms;
        display: flex;
        justify-content: center;
        align-items: center;
        position: absolute;
    }

    #filterByFavoritesButton {
        right: 10px;
    }

    #infoButton {
        left: 10px;
        background-color: white;
    }

    button:hover {
        cursor: pointer;
        scale: 112.5%;
    }

    img {
        height: 20px;
    }
</style>