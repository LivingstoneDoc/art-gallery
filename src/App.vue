<template>
  <div id="app">
    <AppHeader />
    <GalleryMain />
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
};
</script>

<style lang="scss"></style>
