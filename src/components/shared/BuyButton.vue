<template>
  <button
    class="buy-btn"
    :class="buttonClass"
    :disabled="isProcessing"
    @click="handleClick"
  >
    <span v-if="status === 'idle'" class="buy-btn">Купить</span>

    <span v-else-if="status === 'processing'" class="btn-content">
      <svg class="btn-icon spinner" viewBox="0 0 24 24">
        <circle
          cx="12"
          cy="12"
          r="10"
          stroke="currentColor"
          stroke-width="4"
          fill="none"
          opacity="0.25"
        />
        <path
          fill="currentColor"
          d="M12 2a10 10 0 0 1 10 10h-4a6 6 0 0 0-6-6V2z"
          opacity="0.75"
        />
      </svg>
    </span>

    <span v-else-if="status === 'in-cart'" class="btn-content">
      <svg
        class="btn-icon"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
      >
        <path
          d="M20 6L9 17l-5-5"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>
      В корзине
    </span>
  </button>
</template>

<script>
export default {
  name: "BuyButton",

  props: {
    paintingId: {
      type: Number,
      required: true,
    },
  },

  data() {
    return {
      status: "idle",
    };
  },

  computed: {
    buttonClass() {
      return {
        "buy-btn--idle": this.status === "idle",
        "buy-btn--processing": this.status === "processing",
        "buy-btn--in-cart": this.status === "in-cart",
      };
    },

    isProcessing() {
      return this.status === "processing";
    },
  },

  mounted() {
    this.loadCartState();
  },

  methods: {
    handleClick() {
      if (this.status === "processing") return;

      if (this.status === "idle") {
        this.status = "processing";
        this.$emit("processing", this.paintingId);

        setTimeout(() => {
          this.status = "in-cart";
          this.saveToCart();
          this.$emit("added-to-cart", this.paintingId);
        }, 2000);
      } else if (this.status === "in-cart") {
        this.status = "idle";
        this.removeFromCart();
        this.$emit("removed-from-cart", this.paintingId);
      }
    },
    saveToCart() {
      let cart = this.getCart();
      if (!cart.includes(this.paintingId)) {
        cart.push(this.paintingId);
      }
      localStorage.setItem("cart", JSON.stringify(cart));
    },

    removeFromCart() {
      let cart = this.getCart();
      cart = cart.filter((id) => id !== this.paintingId);
      localStorage.setItem("cart", JSON.stringify(cart));
    },

    getCart() {
      const cartData = localStorage.getItem("cart");
      return cartData ? JSON.parse(cartData) : [];
    },

    loadCartState() {
      const cart = this.getCart();
      if (cart.includes(this.paintingId)) {
        this.status = "in-cart";
      }
    },
  },
};
</script>

<style lang="scss" scoped>
@import "@/assets/styles/variables.scss";

.buy-btn {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 118px;
  height: 48px;
  font-family: $main-font;
  font-size: 14px;
  font-weight: 700;
  color: $btn-text;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s;

  &--idle {
    background-color: $ordinary-btn;
    cursor: pointer;

    &:hover {
      background-color: $hover-btn;
    }
  }

  &--processing {
    background-color: $disabled-btn;
    cursor: wait;

    &:hover {
      background-color: $disabled-btn;
    }
  }

  &--in-cart {
    background-color: $in-cart-btn;
    cursor: pointer;

    &:hover {
      background-color: darken($in-cart-btn, 10%);
    }
  }

  &:disabled {
    cursor: not-allowed;
  }
}

.btn-content {
  display: flex;
  align-items: center;
  gap: 8px;
}

.btn-icon {
  width: 20px;
  height: 20px;
  flex-shrink: 0;
}

.spinner {
  animation: rotate 1s linear infinite;
}

@keyframes rotate {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>
