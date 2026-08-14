<template>
  <main class="main">
    <div class="container">
      <h1>Картины эпохи Возрождения</h1>
      <div v-if="filteredPaintings.length > 0" class="cards-grid">
        <PaintingCard
          v-for="painting in filteredPaintings"
          :key="painting.id"
          :painting="painting"
          :is-in-cart="cartIds.includes(painting.id)"
          @open-modal="handleOpenModal"
          @add-to-cart="handleAddToCart"
          @remove-from-cart="handleRemoveFromCart"
        />
      </div>
      <div v-else class="no-results">
        <p>Ничего не найдено по запросу "{{ searchQuery }}"</p>
      </div>
    </div>
    <PaintingModal
      v-if="isModalOpen"
      :painting="selectedPainting"
      :is-in-cart="cartIds.includes(selectedPainting.id)"
      @close="handleCloseModal"
      @add-to-cart="handleAddToCart"
      @remove-from-cart="handleRemoveFromCart"
    />
  </main>
</template>

<script>
import paintingsData from "@/mocks/paintings.json";
import PaintingCard from "@/components/gallery/PaintingCard.vue";
import PaintingModal from "../modals/PaintingModal.vue";

export default {
  name: "GalleryMain",
  components: {
    PaintingCard,
    PaintingModal,
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
      isModalOpen: false,
      selectedPainting: null,
      cartIds: [],
    };
  },
  created() {
    const savedCart = localStorage.getItem("cart");
    if (savedCart) {
      this.cartIds = JSON.parse(savedCart);
    }
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
  methods: {
    handleOpenModal(painting) {
      this.selectedPainting = painting;
      this.isModalOpen = true;
    },
    handleCloseModal() {
      this.isModalOpen = false;
      this.selectedPainting = null;
    },
    handleAddToCart(paintingId) {
      if (!this.cartIds.includes(paintingId)) {
        this.cartIds.push(paintingId);
        localStorage.setItem("cart", JSON.stringify(this.cartIds));
      }
    },
    handleRemoveFromCart(paintingId) {
      this.cartIds = this.cartIds.filter((id) => id !== paintingId);
      localStorage.setItem("cart", JSON.stringify(this.cartIds));
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
