<template>
  <div class="card" :class="{ 'card--sold': painting.sold }">
    <img
      :src="`/images/${painting.image}`"
      :alt="painting.title"
      class="card-image"
    />
    <div class="card-body">
      <h2 class="card-title">
        <span>«{{ painting.title }}»</span>
        <span>{{ painting.author }}</span>
      </h2>
      <div v-if="painting.sold" class="card-sold">Продана на аукционе</div>
      <div v-else class="card-footer">
        <div class="prices">
          <span v-if="painting.oldPrice" class="old-price">
            {{ formatPrice(painting.oldPrice) }} ₽
          </span>
          <span class="current-price">
            {{ formatPrice(painting.price) }} ₽
          </span>
        </div>
        <button class="buy-btn">Купить</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "PaintingCard",
  props: {
    painting: {
      type: Object,
      required: true,
    },
  },
  methods: {
    formatPrice(value) {
      return value.toLocaleString("ru-RU");
    },
  },
};
</script>

<style scoped lang="scss">
@import "@/assets/styles/variables.scss";

.card {
  width: 280px;
  border: 1px solid #e1e1e1;
  background-color: $main-bg;
  &--sold {
    opacity: 0.5;
  }
}

.card-image {
  width: 280px;
  height: 160px;
  object-fit: cover;
  display: block;
}

.card-body {
  padding: 20px 24px 24px 24px;
}

.card-title {
  font-size: 18px;
  font-weight: 400;
  line-height: 27px;
  color: $main-text;
  margin-bottom: 22px;
  height: 54px;
  overflow: hidden;
  display: flex;
  flex-direction: column;

  span {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
}

.card-sold {
  font-size: 16px;
  font-weight: 700;
  color: $main-text;
  text-align: left;
  margin-top: 34px;
}

.card-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.prices {
  display: flex;
  flex-direction: column;
}

.old-price {
  font-size: 14px;
  text-decoration: line-through;
  color: #a0a0a0;
}

.current-price {
  font-size: 16px;
  font-weight: 700;
  color: $main-text;
}

.buy-btn {
  width: 118px;
  height: 48px;
  background-color: $ordinary-btn;
  color: $btn-text;
  border: none;
  font-family: $main-font;
  font-size: 14px;
  font-weight: 700;
  cursor: pointer;
  transition: background-color 0.3s;

  &:hover {
    background-color: $hover-btn;
  }
}
</style>
