<template>
  <header class="header">
    <div class="container">
      <div class="header__top">
        <div class="header__logo">
          <a href="#">
            <img 
              src="/5f07cc5d3db5b2c4c8208e7b32e15896d4664c1d.png" 
              alt="Travel Pulse" 
              class="header__logo-img" 
            />
            <span class="header__logo-text">Travel<br />Pulse</span>
          </a>
        </div>

        <div class="header__search">
          <input type="text" placeholder="Поиск" class="search-input" />
          <button class="search-button">🔍</button>
        </div>

        <span class="header__lang">RUS</span>
        <button class="header__login" @click="openLoginModal">Войти</button>
        <button class="header__icon header__user">👤</button>
        <button class="header__icon header__menu">☰</button>
      </div>

      <div class="header__tabs">
        <button class="header__tab">
          <span class="tab-icon-emoji">🚗</span> Прокат автомобилей
        </button>
        <button class="header__tab">
          <span class="tab-icon-emoji">✈️</span> Авиабилеты
        </button>
        <button class="header__tab">
          <span class="tab-icon-emoji">🛏️</span> Отели
        </button>
      </div>

      <h1 class="header__title">Миллионы выгодных путешествий. Один умный поиск.</h1>

      <div class="search-wrapper">
        <form class="search-form" @submit.prevent="handleSearch">

          <div class="form-group" @click="togglePopup('from')">
            <label>Откуда</label>
            <input type="text" :value="selectedFrom" placeholder="Иркутск (IKT)" readonly />
            <div v-if="activePopup === 'from'" class="popup">
              <div class="popup-list">
                <div
                  v-for="city in fromCities"
                  :key="city.code"
                  class="popup-item"
                  @click.stop="selectCity('from', city)"
                >
                  {{ city.name }} ({{ city.code }})
                </div>
              </div>
            </div>
          </div>

          <div class="form-group" @click="togglePopup('to')">
            <label>Куда</label>
            <input type="text" :value="selectedTo" placeholder="Страна, город..." readonly />
            <div v-if="activePopup === 'to'" class="popup">
              <div class="popup-list">
                <div class="popup-item-link" @click.stop="selectToOption('везде')">Поиск везде</div>
                <div class="popup-item-link" @click.stop="selectToOption('сложный')">Поиск сложного маршрута</div>
              </div>
            </div>
          </div>

          <div class="form-group" @click="togglePopup('depart')">
            <label>Туда</label>
            <input type="text" :value="selectedDepart" placeholder="Выбрать дату" readonly />
            <div v-if="activePopup === 'depart'" class="popup popup-calendar">
              <div class="calendar-header">
                <button @click.stop="changeMonth('depart', -1)">‹</button>
                <span>{{ departMonthName }} {{ departYear }}</span>
                <button @click.stop="changeMonth('depart', 1)">›</button>
              </div>
              <div class="calendar-grid">
                <div class="calendar-weekday">пн</div>
                <div class="calendar-weekday">вт</div>
                <div class="calendar-weekday">ср</div>
                <div class="calendar-weekday">чт</div>
                <div class="calendar-weekday">пт</div>
                <div class="calendar-weekday">сб</div>
                <div class="calendar-weekday">вс</div>
                <div
                  v-for="day in departDays"
                  :key="day.date"
                  class="calendar-day"
                  :class="{ 'calendar-day--selected': day.isSelected }"
                  @click.stop="selectDepartDay(day)"
                >
                  {{ day.day }}
                </div>
              </div>
              <button class="popup-apply" @click.stop="applyDepartDate">Применить</button>
            </div>
          </div>

          <div class="form-group" @click="togglePopup('return')">
            <label>Обратно</label>
            <input type="text" :value="selectedReturn" placeholder="Выбрать дату" readonly />
            <div v-if="activePopup === 'return'" class="popup popup-calendar">
              <div class="calendar-header">
                <button @click.stop="changeMonth('return', -1)">‹</button>
                <span>{{ returnMonthName }} {{ returnYear }}</span>
                <button @click.stop="changeMonth('return', 1)">›</button>
              </div>
              <div class="calendar-grid">
                <div class="calendar-weekday">пн</div>
                <div class="calendar-weekday">вт</div>
                <div class="calendar-weekday">ср</div>
                <div class="calendar-weekday">чт</div>
                <div class="calendar-weekday">пт</div>
                <div class="calendar-weekday">сб</div>
                <div class="calendar-weekday">вс</div>
                <div
                  v-for="day in returnDays"
                  :key="day.date"
                  class="calendar-day"
                  :class="{ 'calendar-day--selected': day.isSelected }"
                  @click.stop="selectReturnDay(day)"
                >
                  {{ day.day }}
                </div>
              </div>
              <button class="popup-apply" @click.stop="applyReturnDate">Применить</button>
            </div>
          </div>

          <div class="form-group" @click="togglePopup('travelers')">
            <label>Путешественники</label>
            <input type="text" :value="selectedTravelers" placeholder="1 взрослый" readonly />
            <div v-if="activePopup === 'travelers'" class="popup popup-travelers">
              <div class="traveler-row">
                <div>
                  <div class="traveler-label">Взрослые</div>
                  <div class="traveler-sub">18 лет и старше</div>
                </div>
                <button @click.stop="changeAdult(-1)">-</button>
                <span>{{ adults }}</span>
                <button @click.stop="changeAdult(1)">+</button>
              </div>
              <div class="traveler-row">
                <div>
                  <div class="traveler-label">Дети</div>
                  <div class="traveler-sub">0 - 17 лет</div>
                </div>
                <button @click.stop="changeChild(-1)">-</button>
                <span>{{ children }}</span>
                <button @click.stop="changeChild(1)">+</button>
              </div>
              <button class="popup-apply" @click.stop="applyTravelers">Применить</button>
            </div>
          </div>

          <div class="form-group" @click="togglePopup('class')">
            <label>Класс</label>
            <input type="text" :value="selectedClass" placeholder="Эконом" readonly />
            <div v-if="activePopup === 'class'" class="popup">
              <div class="popup-header">Класс салона</div>
              <div class="popup-list">
                <div
                  v-for="cls in classOptions"
                  :key="cls"
                  class="popup-item"
                  @click.stop="selectClass(cls)"
                >
                  {{ cls }}
                </div>
              </div>
            </div>
          </div>

          <button type="submit" class="search-form__button">Поиск</button>
        </form>
      </div>
    </div>

    <div v-if="showLoginModal" class="modal-overlay" @click.self="closeLoginModal">
      <div class="modal">
        <button class="modal__close" @click="closeLoginModal">✕</button>
        
        <div class="modal__title">
          <img 
            src="/5f07cc5d3db5b2c4c8208e7b32e15896d4664c1d.png" 
            alt="Travel Pulse" 
            class="modal__logo-img" 
          />
          <span class="modal__title-text">TravelPulse</span>
        </div>
        
        <p class="modal__subtitle">Используйте все свои возможности!</p>
        
        <button class="modal__email-btn">Войти с помощью электронной почты</button>
        
        <div class="modal__divider">
          <span></span>
          <span>или</span>
          <span></span>
        </div>
        
        <div class="modal__social">
          <button class="modal__social-btn">
            <img 
              src="/87cf34858b96ae3aeead66058ed8bcfc7e2e1978.png" 
              alt="Facebook" 
              class="modal__social-icon" 
            />
          </button>
          <button class="modal__social-btn">
            <img 
              src="/7f42d49b6919fba36ba2449fa426a722d691d6c6.png" 
              alt="Google" 
              class="modal__social-icon" 
            />
          </button>
          <button class="modal__social-btn">
            <img 
              src="/042b9d98284a60ff1e53dd66032036cda004c5f7.png" 
              alt="Apple" 
              class="modal__social-icon" 
            />
          </button>
        </div>
        
        <label class="modal__remember">
          <input type="checkbox" v-model="rememberMe" /> Запомнить меня
        </label>
        
        <p class="modal__terms">
          Продолжая, вы принимаете наши <a href="#">Условия предоставления услуг</a> и <a href="#">Политику конфиденциальности</a>.
        </p>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: 'AppHeader',
  data() {
    return {
      fromCities: [
        { name: 'Москва', code: 'SVO' },
        { name: 'Москва', code: 'VKO' },
        { name: 'Москва', code: 'ZIA' },
        { name: 'Санкт-Петербург', code: 'LED' },
        { name: 'Казань', code: 'KZN' },
        { name: 'Сочи', code: 'AER' },
        { name: 'Иркутск', code: 'IKT' },
      ],
      classOptions: ['Эконом', 'Премиум-эконом', 'Бизнес-класс', 'Первый-класс'],
      selectedFrom: 'Иркутск (IKT)',
      selectedTo: '',
      selectedDepart: '',
      selectedReturn: '',
      selectedTravelers: '1 взрослый',
      selectedClass: 'Эконом',
      adults: 1,
      children: 0,
      activePopup: null,
      showLoginModal: false,
      rememberMe: false,
      departDate: null,
      departMonth: new Date().getMonth(),
      departYear: new Date().getFullYear(),
      departSelectedDay: null,
      returnDate: null,
      returnMonth: new Date().getMonth(),
      returnYear: new Date().getFullYear(),
      returnSelectedDay: null,
    };
  },
  computed: {
    departMonthName() {
      const months = ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'];
      return months[this.departMonth];
    },
    returnMonthName() {
      const months = ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'];
      return months[this.returnMonth];
    },
    departDays() {
      return this.buildCalendar(this.departYear, this.departMonth, 'depart');
    },
    returnDays() {
      return this.buildCalendar(this.returnYear, this.returnMonth, 'return');
    },
  },
  methods: {
    togglePopup(type) {
      if (this.activePopup === type) {
        this.activePopup = null;
      } else {
        this.activePopup = type;
        if (type === 'depart' && !this.departSelectedDay) {
          const today = new Date();
          this.departMonth = today.getMonth();
          this.departYear = today.getFullYear();
        }
        if (type === 'return' && !this.returnSelectedDay) {
          const today = new Date();
          this.returnMonth = today.getMonth();
          this.returnYear = today.getFullYear();
        }
      }
    },
    buildCalendar(year, month, type) {
      const firstDay = new Date(year, month, 1);
      const lastDay = new Date(year, month + 1, 0);
      const days = [];
      let dayOfWeek = firstDay.getDay();
      dayOfWeek = dayOfWeek === 0 ? 7 : dayOfWeek;
      for (let i = 1; i < dayOfWeek; i++) {
        const date = new Date(year, month, i - dayOfWeek + 1);
        days.push({
          day: date.getDate(),
          date: date.toISOString().split('T')[0],
          isSelected: false,
          isOtherMonth: true,
        });
      }
      for (let d = 1; d <= lastDay.getDate(); d++) {
        const date = new Date(year, month, d);
        const dateStr = date.toISOString().split('T')[0];
        let isSelected = false;
        if (type === 'depart' && this.departSelectedDay === dateStr) {
          isSelected = true;
        }
        if (type === 'return' && this.returnSelectedDay === dateStr) {
          isSelected = true;
        }
        days.push({
          day: d,
          date: dateStr,
          isSelected,
          isOtherMonth: false,
        });
      }
      const remaining = 42 - days.length;
      for (let i = 1; i <= remaining; i++) {
        const date = new Date(year, month + 1, i);
        days.push({
          day: date.getDate(),
          date: date.toISOString().split('T')[0],
          isSelected: false,
          isOtherMonth: true,
        });
      }
      return days;
    },
    changeMonth(type, delta) {
      if (type === 'depart') {
        this.departMonth += delta;
        if (this.departMonth > 11) {
          this.departMonth = 0;
          this.departYear += 1;
        } else if (this.departMonth < 0) {
          this.departMonth = 11;
          this.departYear -= 1;
        }
      } else {
        this.returnMonth += delta;
        if (this.returnMonth > 11) {
          this.returnMonth = 0;
          this.returnYear += 1;
        } else if (this.returnMonth < 0) {
          this.returnMonth = 11;
          this.returnYear -= 1;
        }
      }
    },
    selectDepartDay(day) {
      this.departSelectedDay = day.date;
    },
    selectReturnDay(day) {
      this.returnSelectedDay = day.date;
    },
    applyDepartDate() {
      if (this.departSelectedDay) {
        const d = new Date(this.departSelectedDay);
        this.selectedDepart = d.toLocaleDateString('ru-RU');
        this.departDate = this.departSelectedDay;
      }
      this.activePopup = null;
    },
    applyReturnDate() {
      if (this.returnSelectedDay) {
        const d = new Date(this.returnSelectedDay);
        this.selectedReturn = d.toLocaleDateString('ru-RU');
        this.returnDate = this.returnSelectedDay;
      }
      this.activePopup = null;
    },
    selectCity(field, city) {
      if (field === 'from') {
        this.selectedFrom = `${city.name} (${city.code})`;
      } else {
        this.selectedTo = `${city.name} (${city.code})`;
      }
      this.activePopup = null;
    },
    selectToOption(option) {
      if (option === 'везде') {
        this.selectedTo = 'Поиск везде';
      } else {
        this.selectedTo = 'Поиск сложного маршрута';
      }
      this.activePopup = null;
    },
    selectClass(cls) {
      this.selectedClass = cls;
      this.activePopup = null;
    },
    changeAdult(delta) {
      const newVal = this.adults + delta;
      if (newVal >= 1) this.adults = newVal;
    },
    changeChild(delta) {
      const newVal = this.children + delta;
      if (newVal >= 0) this.children = newVal;
    },
    applyTravelers() {
      let parts = [];
      if (this.adults > 0) {
        parts.push(`${this.adults} взрослый${this.adults > 1 ? 'х' : ''}`);
      }
      if (this.children > 0) {
        parts.push(`${this.children} ребенок${this.children > 1 ? '' : ''}`);
      }
      this.selectedTravelers = parts.join(', ') || '0 взрослых';
      this.activePopup = null;
    },
    openLoginModal() {
      this.showLoginModal = true;
    },
    closeLoginModal() {
      this.showLoginModal = false;
    },
    handleSearch() {
      // alert('Поиск выполнен!');
    },
  },
};
</script>

<style scoped>
.header {
  background: #1634a1ff;
  padding: 16px 0 30px;
}

.container {
  max-width: 100%;
  margin: 0 auto;
  padding: 0 40px;
}

.header__top {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
  margin-bottom: 16px;
  padding-left: 0;
  margin-left: 0;
}

.header__logo a {
  display: flex;
  align-items: center;
  gap: 10px;
  text-decoration: none;
}

.header__logo-img {
  height: 80px;
  width: auto;
}

.header__logo-text {
  display: flex;
  flex-direction: column;
  font-size: 20px;
  font-weight: 700;
  color: #ffffff;
  line-height: 1.1;
}

.header__search {
  flex: 1;
  max-width: 1000px;
  position: relative;
}

.search-input {
  background: #ffffff;
  border: none;
  border-radius: 20px;
  padding: 8px 40px 8px 18px;
  font-size: 14px;
  color: #1e1e2a;
  width: 100%;
  height: 36px;
  outline: none;
  box-sizing: border-box;
}

.search-input::placeholder {
  color: #999;
}

.search-button {
  position: absolute;
  right: 6px;
  top: 50%;
  transform: translateY(-50%);
  background: transparent;
  border: none;
  font-size: 18px;
  cursor: pointer;
  padding: 4px 8px;
  color: #999;
}

.header__lang {
  color: #ffffff;
  font-size: 14px;
  white-space: nowrap;
}

.header__login {
  background: transparent;
  border: none;
  color: #ffffff;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  padding: 4px 8px;
  transition: opacity 0.2s;
}

.header__login:hover {
  opacity: 0.8;
}

.header__icon {
  background: transparent;
  border: none;
  color: #ffffff;
  font-size: 18px;
  cursor: pointer;
  padding: 4px;
}

.header__menu {
  font-size: 20px;
}

.header__tabs {
  display: flex;
  gap: 12px;
  margin-bottom: 20px;
  padding-left: 25px !important;
  margin-left: 0 !important;
}

.header__tab {
  background: #ffffff;
  border: none;
  border-radius: 20px;
  padding: 6px 16px;
  font-size: 14px;
  font-weight: 500;
  color: #1e1e2a;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 6px;
  transition: 0.2s;
}

.header__tab:hover {
  background: #f0f2f5;
}

.tab-icon-emoji {
  font-size: 18px;
}

.header__title {
  font-size: 36px;
  font-weight: 700;
  color: #ffffff;
  text-align: left;
  margin-bottom: 30px;
  padding-left: 25px !important;
  margin-left: 0 !important;
}

.search-wrapper {
  background: transparent;
  padding: 0;
  box-shadow: none;
  padding-left: 0;
  margin-left: 0;
}

.search-form {
  display: flex;
  align-items: flex-end;
  gap: 12px;
  flex-wrap: wrap;
  padding-left: 25px !important;
  margin-left: 0 !important;
}

.search-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  color: #ffffff;
  padding-bottom: 4px;
}

.form-group {
  flex: 1 1 120px;
  min-width: 100px;
  max-width: 160px;
  display: flex;
  flex-direction: column;
  background: #ffffff;
  border: 1px solid #1e1e2a;
  border-radius: 12px;
  padding: 6px 12px 8px;
  transition: 0.2s;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  cursor: pointer;
  position: relative;
}

.form-group:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.form-group label {
  font-size: 10px;
  font-weight: 600;
  color: #5b5b6b;
  text-transform: uppercase;
  letter-spacing: 0.3px;
  margin-bottom: 0px;
  text-align: left;
}

.form-group input {
  background: transparent;
  border: none;
  padding: 2px 0 4px;
  font-size: 13px;
  color: #1e1e2a;
  width: 100%;
  outline: none;
  font-family: inherit;
  text-align: left;
  cursor: pointer;
}

.form-group input::placeholder {
  color: #999;
  font-size: 12px;
  text-align: left;
}

.popup {
  position: absolute;
  top: calc(100% + 8px);
  left: 0;
  z-index: 100;
  background: #ffffff;
  border: 1px solid #1e1e2a;
  border-radius: 16px;
  padding: 16px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
  min-width: 220px;
  max-height: 380px;
  overflow-y: auto;
}

.popup-header {
  font-size: 14px;
  font-weight: 600;
  color: #1e1e2a;
  margin-bottom: 12px;
  text-align: left;
}

.popup-list {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.popup-item {
  padding: 8px 12px;
  border-radius: 8px;
  font-size: 14px;
  color: #1e1e2a;
  cursor: pointer;
  transition: 0.2s;
  text-align: left;
}

.popup-item:hover {
  background: #f0f2f5;
}

.popup-item-link {
  padding: 8px 12px;
  border-radius: 8px;
  font-size: 14px;
  color: #1634a1ff;
  cursor: pointer;
  transition: 0.2s;
  text-align: left;
  font-weight: 500;
}

.popup-item-link:hover {
  background: #f0f2f5;
}

.popup-calendar {
  min-width: 280px;
  padding: 16px;
}

.calendar-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
  font-weight: 600;
  font-size: 16px;
}

.calendar-header button {
  background: transparent;
  border: none;
  font-size: 20px;
  cursor: pointer;
  padding: 4px 12px;
  border-radius: 6px;
  transition: 0.2s;
}

.calendar-header button:hover {
  background: #f0f2f5;
}

.calendar-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 4px;
  margin-bottom: 12px;
}

.calendar-weekday {
  text-align: center;
  font-size: 12px;
  font-weight: 600;
  color: #5b5b6b;
  padding: 4px 0;
}

.calendar-day {
  text-align: center;
  padding: 6px 0;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  transition: 0.2s;
}

.calendar-day:hover {
  background: #f0f2f5;
}

.calendar-day--selected {
  background: #1634a1ff;
  color: #ffffff;
}

.calendar-day--selected:hover {
  background: #122a7a;
}

.popup-apply {
  width: 100%;
  background: #1634a1ff;
  color: #ffffff;
  border: none;
  border-radius: 8px;
  padding: 8px 16px;
  font-weight: 600;
  font-size: 14px;
  cursor: pointer;
  transition: 0.2s;
  margin-top: 4px;
}

.popup-apply:hover {
  background: #122a7a;
}

.popup-travelers {
  min-width: 220px;
}

.traveler-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  margin-bottom: 10px;
}

.traveler-row div {
  flex: 1;
}

.traveler-label {
  font-weight: 700;
  font-size: 14px;
  color: #1e1e2a;
  text-align: left;
}

.traveler-sub {
  font-size: 12px;
  color: #999;
  text-align: left;
}

.traveler-row button {
  background: #f0f2f5;
  border: none;
  border-radius: 6px;
  width: 28px;
  height: 28px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: 0.2s;
}

.traveler-row button:hover {
  background: #dce0e6;
}

.traveler-row span {
  font-size: 16px;
  font-weight: 600;
  min-width: 24px;
  text-align: center;
}

.search-form__button {
  background: #ff9d1dff;
  color: #ffffff;
  border: none;
  border-radius: 12px;
  padding: 12px 28px;
  font-weight: 600;
  font-size: 16px;
  cursor: pointer;
  height: 48px;
  white-space: nowrap;
  transition: 0.2s;
  align-self: center;
  box-shadow: 0 2px 8px rgba(255, 157, 29, 0.3);
  margin-top: 0;
}

.search-form__button:hover {
  background: #e88a00;
  box-shadow: 0 4px 12px rgba(255, 157, 29, 0.4);
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal {
  background: #ffffff;
  border-radius: 24px;
  padding: 40px 32px;
  width: 400px;
  max-width: 90vw;
  position: relative;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
}

.modal__close {
  position: absolute;
  top: 12px;
  right: 16px;
  background: transparent;
  border: none;
  font-size: 24px;
  cursor: pointer;
  color: #999;
}

.modal__title {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 8px;
  justify-content: flex-start;
}

.modal__logo-img {
  height: 120px;
  width: auto;
}

.modal__title-text {
  font-size: 30px;
  font-weight: 700;
  color: #ff9d1dff;
}

.modal__subtitle {
  font-size: 16px;
  color: #5b5b6b;
  text-align: center;
  margin-bottom: 24px;
}

.modal__email-btn {
  width: 100%;
  padding: 12px;
  background: #1634a1ff;
  color: #ffffff;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  font-size: 16px;
  cursor: pointer;
  margin-bottom: 16px;
}

.modal__email-btn:hover {
  background: #122a7a;
}

.modal__divider {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 16px;
}

.modal__divider span:first-child,
.modal__divider span:last-child {
  flex: 1;
  height: 1px;
  background: #dce0e6;
}

.modal__divider span:nth-child(2) {
  color: #999;
  font-size: 14px;
}

.modal__social {
  display: flex;
  justify-content: center;
  gap: 16px;
  margin-bottom: 16px;
}

.modal__social-btn {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background: #f0f2f5;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: 0.2s;
}

.modal__social-btn:hover {
  background: #dce0e6;
}

.modal__social-icon {
  width: 28px;
  height: 28px;
  object-fit: contain;
}

.modal__remember {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 14px;
  color: #5b5b6b;
  margin-bottom: 16px;
}

.modal__terms {
  font-size: 13px;
  color: #999;
  text-align: center;
}

.modal__terms a {
  color: #1634a1ff;
  text-decoration: none;
}

.modal__terms a:hover {
  text-decoration: underline;
}
</style>
