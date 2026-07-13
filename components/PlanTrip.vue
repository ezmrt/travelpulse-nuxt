<template>
  <section class="plan-trip">
    <div class="container">
      <h2 class="plan-trip__title">Начните планировать поездку</h2>

      <div class="plan-trip__categories">
        <a href="#" class="plan-trip__category" v-for="category in categories" :key="category">
          {{ category }}
        </a>
      </div>

      <div class="plan-trip__links">
        <div class="plan-trip__column" v-for="(column, index) in columns" :key="index">
          <a href="#" class="plan-trip__link" v-for="link in column" :key="link">
            {{ link }}
          </a>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'PlanTrip',
  data() {
    return {
      categories: ['Регион', 'Страна', 'Аэропорт', 'Город'],
      columns: [
        [
          'Дешевые билеты туда-обратно в Париж',
          'Билеты сюда: Канарские острова',
          'Билеты сюда: Каталония'
        ],
        [
          'Рейсы сюда: Токио',
          'Дешевые билеты туда-обратно в Пекин',
          'Билеты сюда: Ереван'
        ],
        [
          'Билеты сюда: Бангкок',
          'Иркутск: дешевые билеты туда-обратно',
          'Рейсы сюда: Дубай'
        ]
      ]
    }
  }
}
</script>

<style scoped>
.plan-trip {
  padding: 40px 0;
  background: #ffffff;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
  text-align: left;
}

.plan-trip__title {
  font-size: 24px;
  font-weight: 700;
  color: #1e1e2a;
  margin-bottom: 16px;
  text-align: left;
}

.plan-trip__categories {
  display: flex;
  gap: 40px;
  margin-bottom: 24px;
  text-align: left;
}

.plan-trip__category {
  font-size: 16px;
  font-weight: 400;
  color: #1e1e2a;
  text-decoration: none;
  transition: 0.2s;
  display: inline-block;
}

.plan-trip__category:hover {
  color: #2e5bff;
}

.plan-trip__links {
  display: flex;
  gap: 40px;
  text-align: left;
}

.plan-trip__column {
  display: flex;
  flex-direction: column;
  gap: 8px;
  flex: 1;
  text-align: left;
}

.plan-trip__link {
  font-size: 14px;
  color: #1e1e2a;
  text-decoration: none;
  transition: 0.2s;
  display: block;
  padding: 0;
  margin: 0;
  text-align: left;
}

.plan-trip__link:hover {
  color: #2e5bff;
}
</style>