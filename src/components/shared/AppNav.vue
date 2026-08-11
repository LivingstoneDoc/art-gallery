<template>
  <ul class="nav">
    <li v-for="(link, index) in links" :key="index">
      <a :href="link.url">{{ link.text }}</a>
    </li>
  </ul>
</template>

<script>
export default {
  name: "AppNav",
  data() {
    return {
      links: [
        { text: "Каталог", url: "#" },
        { text: "Доставка", url: "#" },
        { text: "Оплата", url: "#" },
        { text: "Контакты", url: "#" },
        { text: "О Компании", url: "#" },
      ],
    };
  },
};
</script>

<style scoped lang="scss">
@import "@/assets/styles/variables.scss";
.nav {
  display: flex;
  align-items: center;
  gap: clamp(10px, 2vw, 48px);
  list-style-type: none;
  padding: 0;
  li {
    a {
      text-decoration: none;
      color: inherit;
      font-size: 14px;
      font-weight: 400;
      line-height: 21px;
      white-space: nowrap;
      border-bottom: 1px solid transparent;
      transition: border-color 0.3s ease;
      &:hover {
        border-color: $main-text;
      }
    }
  }
}

@media (max-width: 900px) {
  .nav {
    flex-wrap: wrap;
    justify-content: center;
  }
}
</style>
