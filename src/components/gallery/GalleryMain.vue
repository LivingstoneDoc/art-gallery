<template>
  <main class="main">
    <div class="container">
      <h1>Картины эпохи Возрождения</h1>
      <div v-if="filteredPaintings.length > 0" class="cards-grid">
        <PaintingCard
          v-for="painting in filteredPaintings"
          :key="painting.id"
          :painting="painting"
        />
      </div>
      <div v-else class="no-results">
        <p>Ничего не найдено по запросу "{{ searchQuery }}"</p>
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
  props: {
    searchQuery: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      paintingsList: paintingsData,
    };
  },
  computed: {
    filteredPaintings() {
      if (!this.searchQuery || this.searchQuery.trim() === "") {
        return this.paintingsList;
      }
      const query = this.searchQuery.toLowerCase().trim();

      return this.paintingsList.filter((painting) => {
        return (
          painting.title.toLowerCase().includes(query) ||
          painting.author.toLowerCase().includes(query)
        );
      });
    },
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

.no-results {
  text-align: center;
  padding: 40px 20px;
  color: #9f9f9f;
  font-size: 18px;
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
