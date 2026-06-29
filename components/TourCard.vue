<template>
  <div class="tour-card">
    <img :src="image" :alt="title" class="tour-card__image" />
    <div class="tour-card__content">
      <h3 class="tour-card__title">{{ title }}</h3>
      <p class="tour-card__description">{{ description }}</p>
      <div class="tour-card__rating">
        <span class="stars">★</span>
        <span>{{ rating }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TourCard',
  props: {
    title: String,
    description: String,
    rating: Number,
    image: String
  }
}
</script>

<style scoped>
.tour-card {
  background: #fff;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 16px rgba(0,0,0,0.06);
  transition: 0.3s;
}

.tour-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 24px rgba(0,0,0,0.1);
}

.tour-card__image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.tour-card__content {
  padding: 16px 20px 20px;
}

.tour-card__title {
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 4px;
}

.tour-card__description {
  font-size: 14px;
  color: #5b5b6b;
  margin-bottom: 10px;
}

.tour-card__rating {
  display: flex;
  align-items: center;
  gap: 4px;
  font-weight: 600;
}

.stars {
  color: #f5a623;
}
</style>