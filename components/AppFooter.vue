<template>
  <footer class="footer">
    <div class="container">
      <div class="footer__inner">
        <div class="footer__column">
          <a href="#" class="footer__link">Подробнее</a>
          <a href="#" class="footer__link">Справка</a>
          <a href="#" class="footer__link">О нас</a>
          <a href="#" class="footer__link">Партнерам</a>
          <a href="#" class="footer__link">Поездки</a>
        </div>

        <div class="footer__column">
          <a href="#" class="footer__link">Политика использования файлов cookie</a>
          <a href="#" class="footer__link">Настройки конфиденциальности</a>
          <a href="#" class="footer__link">Политика конфиденциальности</a>
          <a href="#" class="footer__link">Условия предоставления услуг</a>
          <a href="#" class="footer__link">Наши контакты</a>
        </div>

        <div class="footer__column">
          <span class="footer__text">Россия · русский · Р RUB</span>
        </div>
      </div>

      <div class="footer__bottom">
        <p>TravelPulse © 2020-2025. Поиск лучших туров в любой точке мира.</p>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  name: 'AppFooter'
}
</script>

<style scoped>
.footer {
  background: #1634a1ff;
  color: #ffffff;
  padding: 30px 0 15px;
}

.container {
  padding: 0 20px;
}

.footer__inner {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 40px;
  margin-bottom: 20px;
}

.footer__column {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.footer__link {
  color: #ffffff;
  text-decoration: none;
  font-size: 14px;
  transition: opacity 0.2s;
}

.footer__link:hover {
  opacity: 0.8;
}

.footer__text {
  font-size: 14px;
  opacity: 0.9;
}

.footer__bottom {
  border-top: 1px solid rgba(255, 255, 255, 0.2);
  padding-top: 15px;
  text-align: center;
  font-size: 13px;
  opacity: 0.8;
}
</style>