<script setup>
  import { ref, watchEffect, watch, computed } from 'vue';
  import Tile from "./components/Tile.vue";
  import SearchBar from "./components/SearchBar.vue";
  import PageFooter from "./components/PageFooter.vue";
  import SelectedImagePanel from './components/SelectedImagePanel.vue';
  import SiteInformationPopup from './components/SiteInformationPopup.vue';

  const page = ref(1);
  const maxPages = ref(0);
  const selectedImage = ref(-1);
  const data = ref([]);
  const allData = ref([]);
  const searchFilter = ref("");
  const unslicedData = ref([]);
  const favoriteImages = ref([]);
  const filterByFavorites = ref(false);
  const siteInformationPopupVisible = ref(false);

  // configuration
  const siteInformation = "<h1 style='color: red'>Hello!</h1>This is an example website made using the Image Gallery by k327 software.<br><b>I hope you like it :)</b>";
  const jsonUrl = "https://api.slingacademy.com/v1/sample-data/photos?limit=132";
  const allowFavoriting = true;

  watchEffect(async () => {
    allData.value = await(await fetch(jsonUrl)).json();
  })

  watch(() => allData.value + page.value + searchFilter.value + filterByFavorites.value + favoriteImages.value, () => {
    if (allData.value.length != 0) {
      unslicedData.value = allData.value.photos.filter(filterBySearch);
      data.value = unslicedData.value.slice((page.value - 1) * 49, page.value * 49);
      maxPages.value = Math.ceil(unslicedData.value.length / 49);
    }
  })

  const disableMainButtons = computed(() => {
    return selectedImage.value != -1 || siteInformationPopupVisible.value;
  })

  function filterBySearch(e) {
    return (filterByFavorites.value && favoriteImages.value.includes(e.id)) || !filterByFavorites.value ? e.title.toLowerCase().includes(searchFilter.value.trim()) || e.description.toLowerCase().includes(searchFilter.value.trim()) : false;
  }

  function updateSearchFilter(value) {
    searchFilter.value = value.toLowerCase();
    page.value = 1;
  }

  function selectImage(index) {
    selectedImage.value = index + (page.value - 1) * 49;
    document.body.style.overflowY = "hidden";
  }

  function unselectImage() {
    selectedImage.value = -1;
    document.body.style.overflowY = "visible";
  }

  function changeFilterByFavoritesValue() {
    filterByFavorites.value = !filterByFavorites.value;
    page.value = 1;
  }

  function changeSiteInformationVisibility(scrollbarVisibility) {
    siteInformationPopupVisible.value = !siteInformationPopupVisible.value;
    document.body.style.overflowY = scrollbarVisibility;
  }

  function favoriteImage() {
    if (favoriteImages.value.includes(unslicedData.value[selectedImage.value].id)) {
      favoriteImages.value.splice(favoriteImages.value.indexOf(unslicedData.value[selectedImage.value].id), 1);
      if (filterByFavorites.value) {
        unselectImage();
      }
    } else {
      favoriteImages.value.push(unslicedData.value[selectedImage.value].id);
    }
    localStorage.setItem("favoriteImages", favoriteImages.value);
  }

  const favoriteImagesLocalStorage = localStorage.getItem("favoriteImages");
  if (favoriteImagesLocalStorage != null) {
    let number = "";
    for (let i = 0; i < favoriteImagesLocalStorage.length; i++) {
      if (favoriteImagesLocalStorage[i] == ",") {
        favoriteImages.value.push(parseInt(number));
        number = "";
      } else {
        number += favoriteImagesLocalStorage[i];
      }
    }

    if (number != "") {
      favoriteImages.value.push(parseInt(number));
    }
  }
</script>

<template>
  <div>
    <SearchBar @updateSearch="(value) => updateSearchFilter(value)" :filterByFavorites="filterByFavorites" :siteInformation="siteInformation" :allowFavoriting="allowFavoriting" :disable="disableMainButtons" @filterByFavorites="changeFilterByFavoritesValue" @showSiteInformation="changeSiteInformationVisibility('hidden')" />
    <main ref="tilePanel">
      <p id="loadingText" v-if="data.length == 0">
        <img src="@/assets/loading.png" alt="Loading..." v-if="allData.length == 0">
        <span v-else>No images found</span>
      </p>
      <Tile class="tile" v-for="(currentData, index) in data" :data="currentData" @click="selectImage(index)" :disabled="disableMainButtons" />
    </main>
    <PageFooter :page="page" @changePage="(value) => page = value" :maxPages="maxPages" :disable="disableMainButtons" />
    <SelectedImagePanel ref="selectedPhotoPanel" v-if="selectedImage >= 0" :id="selectedImage" :data="selectedImage >= 0 ? unslicedData[selectedImage] : 0" :imageCount="unslicedData.length" :favoriteImages="favoriteImages" :allowFavoriting="allowFavoriting" @unselectImage="unselectImage" @changeSelectedImage="(value) => selectedImage += value" @favoriteImage="favoriteImage" />
    <SiteInformationPopup v-if="siteInformationPopupVisible" :siteInformation="siteInformation" @hideSiteInformation="changeSiteInformationVisibility('visible')" />
  </div>
</template>

<style>
  body {
    color: white;
    margin: 0;
    background-color: rgb(32, 32, 32);
    overflow-x: hidden;
    font-family: Arial, Helvetica, sans-serif;
  }

  div {
    width: 100vw;
    height: auto;
    display: flex;
    flex-direction: column;
  }

  main {
    min-height: calc(100vh - 120px);
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-around;
    align-items: center;
  }

  .tile {
    margin: 7.5px;
  }

  main:first-child {
    margin-top: 10px;
  }

  main:last-child {
    margin-bottom: 10px;
  }

  #loadingText {
    font-size: 150%;
    font-weight: 600;
  }

  #loadingText > img {
    width: 50px;
    height: 50px;
    animation-name: loading;
    animation-duration: 2.5s;
    animation-iteration-count: infinite;
    animation-timing-function: linear;
  }

  @keyframes loading {
    from { rotate: 0deg }
    to { rotate: 360deg }
  }
</style>
