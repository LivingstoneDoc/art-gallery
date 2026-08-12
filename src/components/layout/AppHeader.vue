<template>
  <header class="header">
    <div class="container">
      <div class="inner">
        <AppNav class="header-nav" />
        <div class="search">
          <div class="input-wrapper">
            <input
              ref="searchInput"
              type="text"
              placeholder="Поиск по названию картины"
              v-model="searchQuery"
              @input="onSearch"
              @keyup.enter="onSearch"
            />
            <button
              v-if="searchQuery"
              @click="clearSearch"
              class="clear-btn"
              aria-label="Очистить поиск"
            >
              <svg
                viewBox="0 0 24 24"
                width="16"
                height="16"
                stroke="currentColor"
                stroke-width="2"
                fill="none"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <line x1="18" y1="6" x2="6" y2="18"></line>
                <line x1="6" y1="6" x2="18" y2="18"></line>
              </svg>
            </button>
          </div>
          <button @click="onSearch" class="search-btn">Найти</button>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
import AppNav from "@/components/shared/AppNav.vue";
export default {
  name: "AppHeader",
  components: {
    AppNav,
  },
  data() {
    return {
      searchQuery: "",
    };
  },
  methods: {
    onSearch() {
      this.$emit("search", this.searchQuery);
    },
    clearSearch() {
      this.searchQuery = "";
      this.$emit("search", "");
      this.$nextTick(() => {
        this.$refs.searchInput.focus();
      });
    },
  },
};
</script>

<style scoped lang="scss">
@import "@/assets/styles/variables.scss";
.header {
  height: 97px;
  display: flex;
  align-items: center;
  border-bottom: 1px solid #e1e1e1;
}

.container {
  max-width: 1120px;
  width: 100%;
  padding: 0 20px;
}

.inner {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

.header-nav {
  color: $main-text;
}

.search {
  display: flex;
  justify-content: space-between;
  .input-wrapper {
    position: relative;
    display: flex;
    align-items: center;
    width: 294px;

    input {
      width: 100%;
      height: 48px;
      padding: 14px 36px 14px 16px;
      border: 1px solid #e1e1e1;
      font-family: $main-font;
      font-size: 14px;
      font-weight: 400;
      color: $main-text;
      background-color: $main-bg;
      transition: all 0.2s ease;
      &::placeholder {
        color: #9f9f9f;
      }
      &:focus {
        color: $main-text;
        outline: none;
        border-color: #b5b5b5;
      }
    }

    .clear-btn {
      position: absolute;
      right: 8px;
      top: 50%;
      transform: translateY(-50%);
      display: flex;
      justify-content: center;
      align-items: center;
      color: #b5b5b5;
      background: none;
      border: none;
      border-radius: 50%;
      width: 30px;
      height: 30px;
      padding: 4px 8px;
      cursor: pointer;
      transition: all 0.2s;
    }

    .clear-btn:hover {
      background-color: #f0f0f0;
      color: #333333;
    }

    .clear-btn:active {
      background-color: #e0e0e0;
    }
  }
  .search-btn {
    width: 122px;
    height: 48px;
    padding: 14px 36px;
    background-color: $ordinary-btn;
    color: $btn-text;
    font-family: $main-font;
    font-size: 14px;
    font-weight: 700;
    border: none;
    flex-shrink: 0;
    cursor: pointer;
    transition: background-color 0.3s ease;
    &:hover {
      background-color: $hover-btn;
    }
  }
}

@media (max-width: 1100px) {
  .search .input-wrapper {
    width: 280px;
  }
}

@media (max-width: 900px) {
  .inner {
    flex-direction: column;
    gap: 16px;
  }

  .header {
    height: auto;
    padding: 20px 0;
  }

  .search {
    width: 100%;
    justify-content: center;
  }

  .search .input-wrapper {
    width: 100%;
    max-width: 416px;
  }
}
</style>
