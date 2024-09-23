<script setup>
    const props = defineProps([
        "id",
        "data",
        "imageCount",
        "favoriteImages",
        "allowFavoriting"
    ]);
</script>

<template>
    <section id="main">
        <header>
            <button @click="id == 0 ? {} : $emit('changeSelectedImage', -1)" :class="{ grayedButton: id == 0}" title="Previous image">
                <img src="@/assets/arrow.png" alt="<" id="leftButtonImage">
            </button>
            <p>
                <span :id="!allowFavoriting ? 'title-without-favoriting' : ''" >{{ data.title }}</span>
                <button @click="$emit('favoriteImage')" v-if="allowFavoriting" :style="{ backgroundColor: favoriteImages.includes(data.id) ? 'rgb(255, 200, 0)' : 'rgb(255, 237, 170)' }" :title="favoriteImages.includes(data.id) ? 'Unfavorite image' : 'Favorite image'">
                    <img src="@/assets/star.png" alt="⭐">
                </button>
                <button id="closeButton" @click="$emit('unselectImage')" title="Close">
                    <img src="@/assets/close.png" alt="X">
                </button>
            </p>
            <button @click="id == imageCount - 1 ? {} : $emit('changeSelectedImage', 1)" :class="{ grayedButton: id == imageCount - 1 }" id="rightArrowButton" title="Next image">
                <img src="@/assets/arrow.png" alt=">">
            </button>
        </header>
        <img :src="data.url" :alt="data.description" id="selectedImage">
        <footer>
            <p>{{ data.description }}</p>
        </footer>
    </section>
</template>

<style scoped>
    #main {
        position: fixed;
        width: 100vw;
        height: 100vh;
        background-image: linear-gradient(rgb(0, 0, 0, 0.9), rgb(0, 0, 0, 0.95));
        z-index: 100;
        display: flex;
        justify-content: center;
        align-items: center;
        color: white;
    }

    header, footer {
        height: 60px;
        width: 100vw;
        position: fixed;
    }

    header {
        top: 0;
        display: flex;
        justify-content: space-between;
        align-items: center;
        font-weight: 600;
        font-size: 140%;
        padding: 0 10px;
        box-sizing: border-box;
    }

    footer {
        bottom: 0;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    footer > p {
        font-size: 115%;
        overflow-y: auto;
        text-wrap: wrap;
        text-align: center;
        max-height: 60px;
        display: inline;
        box-sizing: border-box;
        padding: 0 5px;
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
        background-color: white;
    }

    button:hover:not([class="grayedButton"]) {
        cursor: pointer;
        scale: 112.5%;
    }

    .grayedButton {
        background-color: gray !important;
    }

    button > img {
        height: 20px;
    }

    #leftButtonImage {
        rotate: 180deg;
    }

    #selectedImage {
        height: calc(100vh - 140px);
        width: 90vw;
        object-fit: contain;
        z-index: 1;
    }

    p {
        display: flex;
        align-items: center;
    }

    p > button {
        margin-left: 10px;
    }

    #rightArrowButton {
        margin-left: 20px;
    }

    span {
        margin-left: 10px;
        text-align: center;
        display: inline;
        height: 43px;
        margin-top: 17px;
    }

    @media (min-width: 750px) {
        span {
            margin-left: 100px;
        }

        #title-without-favoriting {
            margin-left: 50px;
        }
    }

    #closeButton {
        background-color: rgb(255, 170, 170);
    }
</style>