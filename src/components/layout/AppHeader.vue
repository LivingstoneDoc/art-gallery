<template>
  <header class="header">
    <div class="container">
      <div class="inner">
        <AppNav class="header-nav" />
        <div class="search">
          <input type="text" placeholder="Поиск по названию картины" />
          <button>Найти</button>
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
  input {
    width: 294px;
    height: 48px;
    padding: 14px 16px;
    border: 1px solid #e1e1e1;
    font-family: $main-font;
    font-size: 14px;
    font-weight: 400;
    color: $main-text;
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
  button {
    width: 122px;
    height: 48px;
    padding: 14px 36px;
    background-color: $ordinary-btn;
    color: $btn-text;
    font-family: $main-font;
    font-size: 14px;
    font-weight: 700;
    border: none;
    cursor: pointer;
    transition: background-color 0.3s ease;
    &:hover {
      background-color: $hover-btn;
    }
  }
}

@media (max-width: 1100px) {
  .search input {
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

  .search input {
    width: 100%;
    max-width: 416px;
  }
}
</style>
