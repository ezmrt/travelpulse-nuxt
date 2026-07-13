<template>
  <section class="popular-tours">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">Найти популярные туры</h2>
        <a href="#" class="section-link">Смотреть все →</a>
      </div>
      <div class="tours-grid">
        <TourCard
          v-for="tour in tours"
          :key="tour.id"
          :title="tour.title"
          :description="tour.description"
          :rating="tour.rating"
          :image="tour.image"
        />
      </div>
    </div>
  </section>
</template>

<script>
import TourCard from './TourCard.vue';

export default {
  name: 'PopularTours',
  components: { TourCard },
  data() {
    return {
      tours: [
        { id: 1, title: 'France, Paris', description: 'DisneyLand - экскурсия, питание, отель.', rating: 4.7, image: '/images/7a40393a512a51cca6106fff2ec2f6d346c82e52.jpg' },
        { id: 2, title: 'Italy, Roma', description: 'Coliseum - экскурсия, питание, отель.', rating: 4.2, image: '/images/98aa2ab88e094d8632cbbd8389a31455d15c8f9c.jpg' },
        { id: 3, title: 'Indonesia, Bali', description: 'Tanakh-Lot - экскурсия, питание, отель.', rating: 5.0, image: '/images/582c258e257bf46eb30918e5154a5e8f714f279b.jpg' },
        { id: 4, title: 'Russia, Irkutsk', description: 'Olkhon - экскурсия, питание, отель.', rating: 4.0, image: '/images/bb9b0c207f49d1a2c98168429318321bac7c0fe9.jpg' },
        { id: 5, title: 'China', description: 'The Great Wall of China - экскурсия, питание, отель.', rating: 4.1, image: '/images/9651db3182b7ca0bb6dcb92675e163b3ce3ed50f.jpg' },
        { id: 6, title: 'UAE, Dubai', description: 'Burj Khalifa - экскурсия, питание, отель.', rating: 4.8, image: '/images/12f7d81e6b792fb294ede1d72a9e69f6708bbf89.jpg' }
      ]
    }
  }
}
</script>

<style scoped>
.popular-tours {
  padding: 40px 0;
  background: #f8f9fa;
}

.container {
  padding: 0 20px;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
}

.section-title {
  font-size: 24px;
  font-weight: 700;
  color: #1e1e2a;
}

.section-link {
  color: #2e5bff;
  text-decoration: none;
  font-weight: 500;
}

.tours-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 24px;
}
</style>
