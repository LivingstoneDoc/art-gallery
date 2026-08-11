<template>
  <main class="main">
    <div class="container">
      <h1>Картины эпохи Возрождения</h1>
      <div class="cards-grid">
        <PaintingCard
          v-for="painting in paintingsList"
          :key="painting.id"
          :painting="painting"
        />
      </div>
    </div>
  </main>
</template>

<script>
import paintingsData from "@/mocks/paintings.json";
import PaintingCard from "@/components/gallery/PaintingCard.vue";

export default {
  name: "GalleryMain",
  components: {
    PaintingCard,
  },
  data() {
    return {
      paintingsList: paintingsData,
    };
  },
};
</script>

<style scoped lang="scss">
@import "@/assets/styles/variables.scss";

.main {
  flex: 1;
  padding: 45px 0;
}

h1 {
  font-size: 24px;
  font-weight: 700;
  line-height: 36px;
  color: $main-text;
  margin-bottom: 39px;
}

.cards-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 32px;
}

@media (max-width: 1260px) {
  .main {
    padding: 40px 20px;
  }
}

@media (max-width: 1200px) {
  .cards-grid {
    justify-content: center;
  }
}
</style>
