<template>
  <transition name="modal">
    <div class="modal-overlay" @click.self="$emit('close')">
      <div class="modal-content">
        <button class="modal-close" @click="$emit('close')">&times;</button>
        <div class="modal-body">
          <div class="slider">
            <img
              :src="`/images/${images[currentSlide]}`"
              :alt="painting.title"
              class="slider-image"
            />
            <div class="slider-controls" v-if="images.length > 1">
              <button class="slider-btn prev" @click="prevSlide">
                &#10094;
              </button>
              <button class="slider-btn next" @click="nextSlide">
                &#10095;
              </button>
            </div>
            <div class="slider-dots" v-if="images.length > 1">
              <span
                v-for="(img, index) in images"
                :key="index"
                class="dot"
                :class="{ active: index === currentSlide }"
                @click="currentSlide = index"
              ></span>
            </div>
          </div>
          <div class="info">
            <h2 class="info-title">«{{ painting.title }}»</h2>
            <h3 class="info-author">{{ painting.author }}</h3>
            <p class="info-desc">
              {{ painting.description || "Описание временно отсутствует." }}
            </p>
            <div v-if="painting.sold" class="info-sold">
              Продана на аукционе
            </div>
            <div v-else class="info-price-block">
              <div class="prices">
                <span v-if="painting.oldPrice" class="old-price">
                  {{ formatPrice(painting.oldPrice) }} ₽
                </span>
                <span class="current-price">
                  {{ formatPrice(painting.price) }} ₽
                </span>
              </div>
              <BuyButton
                :painting-id="painting.id"
                :is-in-cart="isInCart"
                @added-to-cart="handleAddedToCart"
                @removed-from-cart="handleRemovedFromCart"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import BuyButton from "../shared/BuyButton.vue";
export default {
  name: "PaintingModal",
  components: {
    BuyButton,
  },
  props: {
    painting: {
      type: Object,
      required: true,
    },
    isInCart: { type: Boolean, default: false },
  },
  data() {
    return {
      currentSlide: 0,
    };
  },
  computed: {
    images() {
      return this.painting.gallery && this.painting.gallery.length > 0
        ? this.painting.gallery
        : [this.painting.image];
    },
  },
  methods: {
    formatPrice(value) {
      return value.toLocaleString("ru-RU");
    },
    nextSlide() {
      this.currentSlide = (this.currentSlide + 1) % this.images.length;
    },
    prevSlide() {
      this.currentSlide =
        (this.currentSlide - 1 + this.images.length) % this.images.length;
    },
    handleAddedToCart(paintingId) {
      this.$emit("add-to-cart", paintingId);
    },
    handleRemovedFromCart(paintingId) {
      this.$emit("remove-from-cart", paintingId);
    },
  },
  mounted() {
    document.body.style.overflow = "hidden";
  },
  beforeDestroy() {
    document.body.style.overflow = "";
  },
};
</script>

<style scoped lang="scss">
@import "@/assets/styles/variables.scss";

.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.3s ease;
}
.modal-enter,
.modal-leave-to {
  opacity: 0;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  padding: 20px;
}

.modal-content {
  background-color: $main-bg;
  max-width: 800px;
  width: 100%;
  position: relative;
  padding: 40px;
  box-shadow: 0px 10px 30px rgba(0, 0, 0, 0.1);
  max-height: 90vh;
  overflow-y: auto;
}

.modal-close {
  position: absolute;
  top: 15px;
  right: 20px;
  font-size: 28px;
  background: none;
  border: none;
  cursor: pointer;
  color: #a0a0a0;
  transition: color 0.2s;

  &:hover {
    color: $main-text;
  }
}

.modal-body {
  display: flex;
  gap: 30px;

  @media (max-width: 768px) {
    flex-direction: column;
  }
}

.slider {
  flex: 1;
  max-width: 400px;
  position: relative;
}

.slider-image {
  width: 100%;
  height: 300px;
  object-fit: contain;
  background-color: #f6f6f6;
  border: 1px solid #e1e1e1;
}

.slider-btn {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(255, 255, 255, 0.8);
  border: none;
  font-size: 18px;
  padding: 10px 15px;
  cursor: pointer;
  transition: background 0.3s;

  &:hover {
    background: white;
  }

  &.prev {
    left: 0;
  }
  &.next {
    right: 0;
  }
}

.slider-dots {
  text-align: center;
  margin-top: 10px;

  .dot {
    display: inline-block;
    width: 10px;
    height: 10px;
    background-color: #e1e1e1;
    border-radius: 50%;
    margin: 0 5px;
    cursor: pointer;

    &.active {
      background-color: $main-text;
    }
  }
}

.info {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.info-title {
  font-size: 24px;
  font-weight: 700;
  color: $main-text;
  margin-bottom: 5px;
}

.info-author {
  font-size: 18px;
  color: $main-text;
  margin-bottom: 20px;
}

.info-desc {
  font-size: 16px;
  line-height: 1.5;
  color: $main-text;
  margin-bottom: 30px;
  flex-grow: 1;
}

.info-sold {
  font-size: 18px;
  font-weight: 700;
  color: #a0a0a0;
}

.info-price-block {
  display: flex;
  align-items: center;
  gap: 32px;
  margin-top: 30px;
  flex-wrap: wrap;
}

.info-price-block .prices {
  display: flex;
  flex-direction: column;
}

.old-price {
  font-size: 14px;
  text-decoration: line-through;
  color: #a0a0a0;
}

.current-price {
  font-size: 24px;
  font-weight: 700;
  color: $main-text;
}

@media (max-width: 768px) {
  .modal-content {
    padding: 45px 20px 20px 20px;
    max-height: 95vh;
  }

  .slider-image {
    height: 220px;
  }

  .modal-body {
    flex-direction: column;
    gap: 20px;
  }

  .modal-close {
    top: 8px;
  }
}
</style>
