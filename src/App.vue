<template>
  <div id="app">
    <AppHeader @search="handleSearch" />
    <GalleryMain :searchQuery="searchQuery" />
    <AppFooter />
  </div>
</template>

<script>
import AppHeader from "./components/layout/AppHeader.vue";
import GalleryMain from "./components/gallery/GalleryMain.vue";
import AppFooter from "./components/layout/AppFooter.vue";

export default {
  name: "App",
  components: {
    AppHeader,
    GalleryMain,
    AppFooter,
  },
  data() {
    return {
      searchQuery: "",
    };
  },
  methods: {
    handleSearch(query) {
      this.searchQuery = query;
    },
  },
};
</script>

<style lang="scss"></style>
