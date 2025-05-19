<template>
  <div class="page-content">
    <h1>Добро пожаловать на мой сайт</h1>
    <p>
      Здесь мог бы быть какой-то важный контент, но тебе выпал судьбоносный
      вопрос.
    </p>
    <img src="./assets/rimg.jpg" alt="Random Image" title="random image" />
    <img src="./assets/image.png" alt="Random Image" title="random image" />
    <img src="./assets/laalal.png" alt="Random Image" title="random image" />
  </div>

  <div v-if="visible" class="overlay">
    <div class="popup" ref="popup">
      <p class="popup-text">Сосал?</p>
      <div class="buttons">
        <button class="yes-btn" @click="closePopup">Да</button>
        <ConfettiExplosion
          v-if="confetti"
          :particleCount="300"
          :force="1"
          :duration="5000"
        />
        <button
          ref="noButton"
          class="no-btn"
          @mouseover="moveButton"
          @click="chaosMode"
        >
          Нет
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, nextTick } from "vue";
import ConfettiExplosion from "vue-confetti-explosion";

const visible = ref(true);
const confetti = ref(false);
const noButton = ref(null);
const popup = ref(null);
const body = document.body;

const closePopup = async () => {
  confetti.value = true;
  await nextTick();
  setTimeout(() => {
    confetti.value = false;
    visible.value = false;
  }, 5000);
};

const moveButton = async () => {
  if (!noButton.value || !popup.value) return;

  await nextTick();

  const popupRect = popup.value.getBoundingClientRect();
  const buttonRect = noButton.value.getBoundingClientRect();

  const maxX = popupRect.width - buttonRect.width - 20;
  const maxY = popupRect.height - buttonRect.height - 20;
  const minShift = 30;

  let newX = Math.random() * maxX;
  let newY = Math.random() * maxY;

  const currentX = parseFloat(noButton.value.style.left) || 0;
  const currentY = parseFloat(noButton.value.style.top) || 0;

  if (Math.abs(newX - currentX) < minShift) {
    newX += minShift;
  }
  if (Math.abs(newY - currentY) < minShift) {
    newY += minShift;
  }

  noButton.value.style.position = "absolute";
  noButton.value.style.left = `${newX}px`;
  noButton.value.style.top = `${newY}px`;
};

const chaosMode = () => {
  body.style.transition = "transform 0.5s linear";
  body.style.transform = "rotate(180deg)";
  closePopup();
  setTimeout(() => {
    setInterval(() => {
      body.style.transform = `rotate(${Date.now() % 360}deg)`;
    }, 50);
  }, 500);
};
</script>

<style scoped>
body {
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  margin: 0;
  padding: 0;
  background: #f4f4f4;
  text-align: center;
}

.page-content {
  max-width: 800px;
  margin: 50px auto;
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.page-content img {
  width: 100%;
  border-radius: 10px;
  margin-top: 10px;
}

.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.popup {
  background: white;
  padding: 20px;
  border-radius: 12px;
  text-align: center;
  position: relative;
  width: 320px;
  height: 170px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
  animation: fadeIn 0.3s ease-out;
  gap: 30px;
}

.popup-text {
  font-size: 48px;
  font-weight: 600;
  margin: 0;
}

.buttons {
  position: relative;
  width: 100%;
  height: 50px;
  display: flex;
  gap: 50px;
  justify-content: center;
  padding: 0 20px;
}

.yes-btn,
.no-btn {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.2s, transform 0.2s;
}

.yes-btn:hover {
  background: #0056b3;
}

.no-btn {
  position: relative;
  transition: 0.1s;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: scale(0.9);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}
</style>
