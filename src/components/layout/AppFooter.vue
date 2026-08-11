<template>
  <footer class="footer">
    <div class="container">
      <div class="inner">
        <AppNav class="footer-nav" />
        <div class="contacts">
          <div class="phone">
            <img src="@/assets/images/icons/phone.svg" alt="phone" />
            <a href="tel:+78125555555">+7 (812) 555-55-55</a>
          </div>
          <div class="address">
            <img src="@/assets/images/icons/address.svg" alt="address" />
            <div>г. Санкт-Петербург, ул. Ефимова, 3</div>
          </div>
        </div>
      </div>
    </div>
  </footer>
</template>

<script>
import AppNav from "@/components/shared/AppNav.vue";
export default {
  name: "AppFooter",
  components: {
    AppNav,
  },
};
</script>

<style scoped lang="scss">
@import "@/assets/styles/variables.scss";
.footer {
  height: 96px;
  display: flex;
  align-items: center;
  background-color: #eceaea;
}

.container {
  max-width: 1120px;
  padding: 0 20px;
}

.inner {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: clamp(30px, 4vw, 130px);
}

.footer-nav {
  color: $footer-text;
}

.contacts {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: clamp(20px, 3vw, 66px);

  .phone,
  .address {
    display: flex;
    align-items: center;
    gap: 8px;
    a {
      text-decoration: none;
      color: $footer-text;
      font-size: 14px;
      font-weight: 400;
      line-height: 21px;
    }

    a {
      border-bottom: 1px solid transparent;
      transition: border-color 0.3s ease;
      &:hover {
        border-color: $footer-text;
      }
    }
  }
}

@media (max-width: 900px) {
  .inner {
    flex-direction: column;
    gap: 24px;
  }

  .footer {
    height: auto;
    padding: 20px 0;
  }
}

@media (max-width: 600px) {
  .contacts {
    flex-direction: column;
    align-items: center;
    gap: 16px;
  }
}
</style>
