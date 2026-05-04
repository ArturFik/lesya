<template>
  <div class="invitation-card">
    <!-- Фотография -->
    <div class="photo-container">
      <img :src="photoUrl" alt="Савелий и Ляйсан" class="wedding-photo" />
    </div>

    <div class="timer-container">
      <div class="timer-title">До нашей свадьбы осталось</div>
      <div class="timer">
        <div class="timer-block">
          <div class="timer-number">{{ days }}</div>
          <div class="timer-label">дней</div>
        </div>
        <div class="timer-separator">:</div>
        <div class="timer-block">
          <div class="timer-number">{{ hours }}</div>
          <div class="timer-label">часов</div>
        </div>
        <div class="timer-separator">:</div>
        <div class="timer-block">
          <div class="timer-number">{{ minutes }}</div>
          <div class="timer-label">минут</div>
        </div>
        <div class="timer-separator">:</div>
        <div class="timer-block">
          <div class="timer-number">{{ seconds }}</div>
          <div class="timer-label">секунд</div>
        </div>
      </div>
    </div>

    <div class="content">
      <div class="names">
        <h1>
          Савелий
          <span class="ampersand">&</span>
          Ляйсан
        </h1>
      </div>

      <div style="text-align: center">
        <div class="invite-line">приглашают вас на свадьбу</div>
      </div>

      <div class="message-text">
        <p>Мы рады пригласить вас на самый важный день в нашей жизни.</p>
        <p class="highlight-wish">
          Собрать всех близких в одном месте — это и есть наше главное желание.<br />
          Будем счастливы разделить с вами этот день!
        </p>
      </div>

      <div class="details">
        <div class="detail-item">
          <span class="detail-label">📅 Когда:</span>
          <span class="detail-value"><strong>3 июля 2026 в 15:00</strong></span>
        </div>
        <div class="detail-item">
          <span class="detail-label">📍 Где:</span>
          <span class="detail-value">ул. Новгородская, 58/1</span>
        </div>
        <div class="detail-item">
          <span class="detail-label">Возраст:</span>
          <span class="detail-value"><strong>18+</strong></span>
        </div>
      </div>

      <div class="program-section">
        <h3>Программа дня</h3>
        <div class="program">
          <div class="event-block">
            <div class="event-title">Сбор гостей в 15:00</div>
          </div>
          <div class="arrow-down" />

          <div class="event-block">
            <div class="event-title">Фуршет и фотосессия</div>
          </div>
          <div class="arrow-down" />

          <div class="event-block">
            <div class="event-title">
              Торжественная церемония <br />в 16:00
            </div>
          </div>
          <div class="arrow-down" />

          <div class="event-block">
            <div class="event-title">Банкет в 17:00</div>
          </div>
        </div>
      </div>

      <div class="dresscode-section">
        <h3>Дресс-код</h3>
        <div class="dresscode-desc">
          Чтобы наш праздник стал более гармоничным и изысканным, мы будем
          рады, если вы придержитесь данной цветовой палитры.
        </div>
        <div class="color-palette">
          <div
            v-for="color in dressColors"
            :key="color"
            class="color-item"
          >
            <div
              class="color-circle"
              :style="{ background: color }"
            />
          </div>
        </div>
      </div>

      <div class="gift-section">
        <h3>О подарках</h3>
        <div class="gift-desc">
          <p>
            Главное для нас — разделить этот день с вами.<br />
            Ваше присутствие, тёплые слова и искренние улыбки — лучшее, что
            может быть в этот праздник.
          </p>
          <p class="gift-option">
            Если вы захотите порадовать нас,<br />
            вместо цветов и традиционных подарков мы будем благодарны подарку в
            конверте, который поможет осуществить наши планы и мечты.
          </p>
        </div>
      </div>

      <div class="message-text">
        <p>Пожалуйста, подтвердите своё присутствие ответным сообщением в течение недели.</p>
      </div>

      <div class="ornament-bottom" />
      <div class="footer-note">С нетерпением ждём встречи!</div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';
import photo from '../public/photo.png';

const dressColors = ref(['#59351F', '#002F55', '#B0C4DE', '#DABDAB', '#E6E6FA', '#DDBEC3']);
const photoUrl = ref(photo);

const getCurrentUfaTime = () => {
  const now = new Date();
  const ufaTime = new Date(now.getTime() + (5 * 60 * 60 * 1000));
  return ufaTime;
};

const targetDate = new Date('2026-07-03T10:00:00Z');

const now = ref(getCurrentUfaTime());

let timerInterval = null;

const days = computed(() => {
  const diff = targetDate.getTime() - now.value.getTime();
  if (diff <= 0) return '00';
  return Math.floor(diff / (1000 * 60 * 60 * 24)).toString().padStart(2, '0');
});

const hours = computed(() => {
  const diff = targetDate.getTime() - now.value.getTime();
  if (diff <= 0) return '00';
  const totalHours = Math.floor(diff / (1000 * 60 * 60));
  return (totalHours % 24).toString().padStart(2, '0');
});

const minutes = computed(() => {
  const diff = targetDate.getTime() - now.value.getTime();
  if (diff <= 0) return '00';
  const totalMinutes = Math.floor(diff / (1000 * 60));
  return (totalMinutes % 60).toString().padStart(2, '0');
});

const seconds = computed(() => {
  const diff = targetDate.getTime() - now.value.getTime();
  if (diff <= 0) return '00';
  const totalSeconds = Math.floor(diff / 1000);
  return (totalSeconds % 60).toString().padStart(2, '0');
});

const updateTimer = () => {
  now.value = getCurrentUfaTime();
};

onMounted(() => {
  updateTimer();
  timerInterval = setInterval(updateTimer, 1000);
});

onUnmounted(() => {
  if (timerInterval) clearInterval(timerInterval);
});
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.invitation-card {
  max-width: 680px;
  width: 100%;
  background: #fffef7;
  border-radius: 32px;
  overflow: hidden;
  transition: all 0.2s ease;
  margin: 0 auto;
}

/* Фотография */
.photo-container {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 24px 24px 16px 24px;
}

.wedding-photo {
  width: 100%;
  max-height: 360px;
  object-fit: cover;
  border-radius: 28px;
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  border: 3px solid #fff8f0;
}

.wedding-photo:hover {
  transform: scale(1.01);
  box-shadow: 0 20px 30px rgba(0, 0, 0, 0.12);
}

/* Таймер */
.timer-container {
  background: linear-gradient(135deg, #fffbff, #fff9f2);
  margin: 0 24px 24px 24px;
  padding: 24px 20px;
  border-radius: 28px;
  border: 1px solid #f0e1d2;
  box-shadow: 0 6px 14px rgba(0, 0, 0, 0.04);
  text-align: center;
}

.timer-title {
  font-family: 'Montserrat Alternates', sans-serif;
  font-size: 1rem;
  font-weight: 500;
  color: #a47148;
  letter-spacing: 2px;
  text-transform: uppercase;
  margin-bottom: 16px;
}

.timer {
  display: flex;
  justify-content: center;
  align-items: baseline;
  gap: 12px;
  flex-wrap: wrap;
}

.timer-block {
  text-align: center;
  background: #fffcf7;
  padding: 12px 16px;
  border-radius: 24px;
  min-width: 80px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.03);
  border: 1px solid #eedbcb;
}

.timer-number {
  font-family: 'Playfair Display', serif;
  font-size: 2.8rem;
  font-weight: 600;
  color: #593512;
  line-height: 1.1;
  letter-spacing: 2px;
}

.timer-label {
  font-family: 'Montserrat Alternates', sans-serif;
  font-size: 0.7rem;
  font-weight: 500;
  color: #b47c48;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-top: 6px;
}

.timer-separator {
  font-family: 'Playfair Display', serif;
  font-size: 2.5rem;
  font-weight: 500;
  color: #c8a27a;
  margin: 0 2px;
}

.timezone-note {
  font-family: 'Montserrat Alternates', sans-serif;
  font-size: 0.7rem;
  color: #a28776;
  margin-top: 12px;
  font-style: italic;
  letter-spacing: 0.5px;
}

.content {
  padding: 0 32px 48px;
}

.names {
  text-align: center;
  margin-bottom: 24px;
  letter-spacing: 2px;
}

.names h1 {
  font-family: 'Playfair Display', serif;
  font-weight: 500;
  font-size: 3.2rem;
  color: #593512;
  margin-bottom: 8px;
  font-style: italic;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.02);
  word-break: keep-all;
}

.ampersand {
  font-family: 'Playfair Display', serif;
  font-size: 2.8rem;
  font-weight: 500;
  color: #593512;
  display: inline-block;
  margin: 0 6px;
  transform: scale(1.1);
  font-style: italic;
}

.invite-line {
  text-align: center;
  font-size: 1rem;
  font-weight: 400;
  color: #a47148;
  letter-spacing: 2px;
  text-transform: uppercase;
  border-top: 1px solid #eedbcb;
  border-bottom: 1px solid #eedbcb;
  display: inline-block;
  width: auto;
  padding: 8px 20px;
  margin: 0 auto 28px;
  font-family: 'Montserrat Alternates', sans-serif;
  background: #fefaf5;
  border-radius: 60px;
}

.message-text {
  text-align: center;
  color: #4a2e1a;
  font-weight: 400;
  line-height: 1.6;
  margin-bottom: 36px;
  font-size: 1rem;
  max-width: 460px;
  margin-left: auto;
  margin-right: auto;
}

.message-text p {
  margin-bottom: 14px;
}

.message-text .highlight-wish {
  font-weight: 500;
  color: #a15e2c;
  font-style: normal;
}

.details {
  background: #fef9f2;
  padding: 18px 20px;
  border-radius: 28px;
  margin-bottom: 32px;
  border: 1px solid #f0e1d2;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.02);
}

.detail-item {
  display: flex;
  align-items: baseline;
  justify-content: center;
  flex-wrap: wrap;
  gap: 8px;
  font-size: 1.1rem;
  margin-bottom: 12px;
  font-family: 'Montserrat Alternates', sans-serif;
}

.detail-item:last-child {
  margin-bottom: 0;
}

.detail-label {
  font-weight: 500;
  color: #b47c48;
  letter-spacing: 0.5px;
  font-size: 1rem;
}

.detail-value {
  font-weight: 400;
  color: #2f241b;
  font-size: 1.05rem;
}

.detail-value strong {
  font-weight: 500;
  color: #8b5a2b;
}

.dresscode-section {
  margin-bottom: 32px;
  background: linear-gradient(135deg, #fffcf8, #fff7f0);
  border-radius: 28px;
  padding: 24px 20px;
  border: 1px solid #eedbcb;
}

.dresscode-section h3 {
  font-family: 'Playfair Display', serif;
  font-weight: 500;
  font-size: 1.6rem;
  color: #a26e3e;
  text-align: center;
  margin-bottom: 16px;
  letter-spacing: 1px;
  font-style: italic;
}

.dresscode-desc {
  text-align: center;
  color: #56443f;
  font-size: 0.95rem;
  line-height: 1.6;
  margin-bottom: 24px;
  font-weight: 400;
  font-family: 'Montserrat Alternates', sans-serif;
}

.color-palette {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 12px;
  margin-top: 20px;
}

.color-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
}

.color-circle {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s ease;
}

.color-circle:hover {
  transform: scale(1.1);
}

.program-section {
  margin-top: 12px;
}

.program-section h3 {
  font-family: 'Playfair Display', serif;
  font-weight: 500;
  font-size: 1.6rem;
  color: #a26e3e;
  text-align: center;
  margin-bottom: 24px;
  letter-spacing: 1px;
  font-style: italic;
}

.program {
  max-width: 480px;
  width: 100%;
  margin: 0 auto;
  padding: 8px 0 12px;
  text-align: center;
}

.event-block {
  margin: 8px 0;
}

.event-title {
  font-size: 1.35rem;
  font-weight: 500;
  color: #5c2a00;
  line-height: 1.3;
  font-family: 'Montserrat Alternates', sans-serif;
  background: linear-gradient(135deg, #fff8f0, #fffcf7);
  display: inline-block;
  padding: 8px 22px;
  border-radius: 60px;
  letter-spacing: 0.3px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.02);
  backdrop-filter: blur(2px);
}

.arrow-down {
  font-size: 0;
  line-height: 1;
  margin: 2px 0;
  display: block;
}

.arrow-down::before {
  content: '❧';
  font-size: 32px;
  display: inline-block;
  transform: rotate(90deg);
  color: #cfaa85;
  font-weight: 400;
}

.program .event-block:last-child .arrow-down {
  display: none;
}

.gift-section {
  margin-bottom: 32px;
  background: linear-gradient(135deg, #fffcf8, #fff7f0);
  border-radius: 28px;
  padding: 24px 20px;
  border: 1px solid #eedbcb;
  text-align: center;
}

.gift-section h3 {
  font-family: 'Playfair Display', serif;
  font-weight: 500;
  font-size: 1.6rem;
  color: #a26e3e;
  text-align: center;
  margin-bottom: 16px;
  letter-spacing: 1px;
  font-style: italic;
}

.gift-desc {
  color: #56443f;
  font-size: 0.95rem;
  line-height: 1.7;
  font-family: 'Montserrat Alternates', sans-serif;
}

.gift-desc p {
  margin-bottom: 14px;
}

.gift-desc p:last-child {
  margin-bottom: 0;
}

.gift-option {
  font-weight: 500;
  color: #8b645a;
  background: rgba(228, 199, 183, 0.3);
  display: inline-block;
  padding: 6px 18px;
  border-radius: 40px;
  margin: 8px 0;
}

.ornament-bottom {
  height: 4px;
  background: linear-gradient(90deg, #e7cfb8, #c8a27a, #e7cfb8);
  width: 80%;
  margin: 20px auto 0;
  border-radius: 4px;
  opacity: 0.5;
}

.footer-note {
  text-align: center;
  font-size: 1.1rem;
  color: #bb9a78;
  font-family: 'Montserrat Alternates', sans-serif;
  margin-top: 28px;
  letter-spacing: 0.6px;
}

@media (max-width: 520px) {
  .content {
    padding: 0 20px 36px;
  }
  .photo-container {
    padding: 20px 20px 12px 20px;
  }
  .timer-container {
    margin: 0 20px 20px 20px;
    padding: 18px 16px;
  }
  .timer-block {
    padding: 6px 12px;
    min-width: 60px;
  }
  .timer-number {
    font-size: 2rem;
  }
  .timer-label {
    font-size: 0.6rem;
  }
  .timer-separator {
    font-size: 1.8rem;
  }
  .names h1 {
    font-size: 2.4rem;
  }
  .event-title {
    font-size: 1.1rem;
    padding: 6px 16px;
  }
  .detail-item {
    font-size: 0.95rem;
    flex-direction: column;
    align-items: center;
    gap: 4px;
  }
  .invite-line {
    font-size: 0.8rem;
  }
  .color-circle {
    width: 40px;
    height: 40px;
  }
}
</style>