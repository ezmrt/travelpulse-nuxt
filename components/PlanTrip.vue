<template>
  <section class="plan-trip">
    <div class="container">
      <h2 class="section-title">Начните планировать поездку</h2>
      <div class="plan-trip__grid">
        <div class="plan-trip__column" v-for="(column, index) in columns" :key="index">
          <h4>{{ column.title }}</h4>
          <ul>
            <li v-for="link in column.links" :key="link">
              <a href="#">{{ link }}</a>
            </li>
          </ul>
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
      columns: [
        {
          title: 'Регион',
          links: [
            'Дешевые билеты туда-обратно в Париж',
            'Билеты сюда: Канарские острова',
            'Билеты сюда: Каталония'
          ]
        },
        {
          title: 'Страна',
          links: [
            'Рейсы сюда: Токио',
            'Дешевые билеты туда-обратно в Пекин',
            'Билеты сюда: Ереван'
          ]
        },
        {
          title: 'Аэропорт',
          links: [
            'Билеты сюда: Бангкок',
            'Иркутск: дешевые билеты туда-обратно',
            'Рейсы сюда: Дубай'
          ]
        },
        {
          title: 'Город',
          links: [
            'Подробнее',
            'О нас',
            'Партнерам',
            'Поездки'
          ]
        }
      ]
    }
  }
}
</script>

<style scoped>
.plan-trip {
  padding: 40px 0;
  background: #fff;
}
.section-title {
  font-size: 24px;
  font-weight: 700;
  margin-bottom: 24px;
  color: #1e1e2a;
}
.plan-trip__grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 30px;
}
.plan-trip__column h4 {
  font-size: 16px;
  font-weight: 600;
  margin-bottom: 12px;
}
.plan-trip__column ul {
  list-style: none;
}
.plan-trip__column li {
  margin-bottom: 8px;
}
.plan-trip__column a {
  color: #5b5b6b;
  text-decoration: none;
  font-size: 14px;
}
.plan-trip__column a:hover {
  color: #2e5bff;
}
</style>