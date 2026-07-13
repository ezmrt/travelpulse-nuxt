<template>
  <section class="destinations">
    <div class="container">
      <h2 class="destinations__title">ПОПУЛЯРНЫЕ НАПРАВЛЕНИЯ</h2>

      <div class="destinations__slider-wrapper">
        <button class="destinations__arrow destinations__arrow--left" @click="scrollLeft">
          ‹
        </button>

        <div class="destinations__slider" ref="slider">
          <div
            class="destinations__item"
            v-for="city in cities"
            :key="city.name"
          >
            <img :src="city.image" :alt="city.name" class="destinations__image" />
            <span class="destinations__name">{{ city.name }}</span>
          </div>
        </div>

        <button class="destinations__arrow destinations__arrow--right" @click="scrollRight">
          ›
        </button>
      </div>

      <div class="destinations__dots">
        <span
          v-for="(dot, index) in dots"
          :key="index"
          class="destinations__dot"
          :class="{ 'destinations__dot--active': index === currentPage }"
          @click="scrollToPage(index)"
        ></span>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'PopularDestinations',
  data() {
    return {
      currentPage: 0,
      itemsPerPage: 4,
      cities: [
        { name: 'Париж', image: '/images/eaa255f4fd9a48dcedb6ba913c1fd03c3f2520a2.png' },
        { name: 'Рим', image: '/images/9aea977b0df1a0b23180692be43e1581cb6e6c1c.png' },
        { name: 'Бали', image: '/images/be487c84ed73cbb733f7e9259500018c71aa94dc.png' },
        { name: 'Байкал', image: '/images/1d4d305ee0eea20fac8ff08913324b92fe3d970a.png' },
        { name: 'Лондон', image: '/images/9d8a25bcb6d03e5af531cb6a15571954.png' },
        { name: 'Токио', image: '/images/TOKIO.png' },
        { name: 'Нью-Йорк', image: '/images/592642d3fa8520ccd61a2539f49f4f37.png' },
        { name: 'Дубай', image: '/images/dubai.png' },
      ],
    };
  },
  computed: {
    totalPages() {
      return Math.ceil(this.cities.length / this.itemsPerPage);
    },
    dots() {
      return Array.from({ length: this.totalPages }, (_, i) => i);
    },
  },
  mounted() {
    const slider = this.$refs.slider;
    if (slider) {
      slider.addEventListener('scroll', this.updatePageFromScroll);
    }
    this.updatePageFromScroll();
  },
  beforeUnmount() {
    const slider = this.$refs.slider;
    if (slider) {
      slider.removeEventListener('scroll', this.updatePageFromScroll);
    }
  },
  methods: {
    updatePageFromScroll() {
      const slider = this.$refs.slider;
      if (!slider) return;
      const itemWidth = slider.querySelector('.destinations__item')?.offsetWidth || 0;
      const gap = parseInt(getComputedStyle(slider).gap) || 20;
      const scrollLeft = slider.scrollLeft;
      const page = Math.round(scrollLeft / ((itemWidth + gap) * this.itemsPerPage));
      if (page >= 0 && page < this.totalPages) {
        this.currentPage = page;
      }
    },
    scrollToPage(page) {
      const slider = this.$refs.slider;
      if (!slider) return;
      const itemWidth = slider.querySelector('.destinations__item')?.offsetWidth || 0;
      const gap = parseInt(getComputedStyle(slider).gap) || 20;
      const scrollPosition = page * (itemWidth + gap) * this.itemsPerPage;
      slider.scrollTo({
        left: scrollPosition,
        behavior: 'smooth',
      });
      this.currentPage = page;
    },
    scrollLeft() {
      const newPage = Math.max(0, this.currentPage - 1);
      this.scrollToPage(newPage);
    },
    scrollRight() {
      const newPage = Math.min(this.totalPages - 1, this.currentPage + 1);
      this.scrollToPage(newPage);
    },
  },
};
</script>

<style scoped>
.destinations {
  padding: 40px 0;
  background: #ffffff;
}

.container {
  padding: 0 20px;
}

.destinations__title {
  font-size: 24px;
  font-weight: 700;
  color: #1e1e2a;
  text-align: center;
  margin-bottom: 24px;
}

.destinations__slider-wrapper {
  position: relative;
  display: flex;
  align-items: center;
  gap: 12px;
}

.destinations__slider {
  display: flex;
  gap: 100px;
  overflow-x: hidden;
  scroll-snap-type: x mandatory;
  padding: 8px 0 16px;
  scrollbar-width: none;
  flex: 1;
}

.destinations__slider::-webkit-scrollbar {
  display: none;
}

.destinations__item {
  flex: 0 0 160px;
  text-align: center;
  scroll-snap-align: start;
  transition: 0.3s;
}

.destinations__item:hover {
  transform: translateY(-4px);
}

.destinations__image {
  width: 160px;
  height: 160px;
  border-radius: 50%;
  object-fit: cover;
  display: block;
  margin: 0 auto;
  transition: transform 0.3s ease;
}

.destinations__item:hover .destinations__image {
  transform: scale(1.05);
}

.destinations__name {
  display: block;
  padding: 12px 0 0;
  font-weight: 600;
  font-size: 16px;
  color: #1e1e2a;
}

.destinations__arrow {
  background: #ffffff;
  border: 1px solid #dce0e6;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  font-size: 24px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: 0.3s;
  flex-shrink: 0;
  color: #1e1e2a;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
}

.destinations__arrow:hover {
  background: #1634a1ff;
  color: #ffffff;
  border-color: #1634a1ff;
}

.destinations__arrow--left {
  margin-right: 4px;
}

.destinations__arrow--right {
  margin-left: 4px;
}

.destinations__dots {
  display: flex;
  justify-content: center;
  gap: 8px;
  margin-top: 16px;
}

.destinations__dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #dce0e6;
  cursor: pointer;
  transition: 0.3s;
}

.destinations__dot--active {
  background: #1634a1ff;
  width: 20px;
  border-radius: 4px;
}
</style>
