<template>
  <div 
    class="yamaguchi-slide" 
    :class="`slide-${slideIndex}`"
    :style="{ backgroundImage: slideIndex === 2 && isMobile ? 'none' : `url(${bgImage})` }"
  >
    <div v-if="slideIndex === 0" class="slide-content">
      
      <div class="title">
        ФЕН и СТАЙЛЕР<br>в уникальном дизайне
      </div>
      
      <div class="badge">2 в 1</div>

      <div class="side-blocks">
        <div class="side-item left">
          <div class="side-label label-dark">Фен</div>
          <img src="@/assets/image/slider-1-fen.jpg" alt="Фен" class="side-image" />
        </div>
        
        <div class="side-item right">
          <div class="side-label label-light">Стайлер</div>
          <img src="@/assets/image/slider-1-styler.jpg" alt="Стайлер" class="side-image" />
        </div>
      </div>
      
    </div>

    <div v-if="slideIndex === 1" class="slide-content second-slide">
      
      <div class="coffee-circle">
        <img src="@/assets/image/coffee-star.svg" alt="coffee" class="coffee-img" />
        <span class="coffee-text">Не тяжелее<br>чашки кофе</span>
      </div>

      <div class="second-right">
        <div class="title-second">
          СУПЕР-<br>ЛЁГКИЙ
        </div>
        
        <div class="description">
          Уменьшает усталость рук<br>
          при сушке волос, делая процедуру<br>
          удобной и приятной даже после<br>
          напряжённого рабочего дня.
        </div>
        
        <div class="badge-second">
          500 г
        </div>
      </div>

    </div>

    <div v-if="slideIndex === 2" class="slide-content third-slide">
      
      <div v-if="!isMobile" class="third-text-block">
        <div class="title-third">
          Для ценителей комфорта<br>и качества в уходе за волосами
        </div>
        <div class="subtitle-third">
          Бесщеточный мотор BLDC
        </div>
      </div>

      <div v-if="!isMobile" class="power-circle">
        <img src="@/assets/image/coffee-star.svg" alt="power" class="coffee-img" />
        <span class="power-text">Салонная<br>мощность</span>
      </div>

      <div v-if="isMobile" class="mobile-third-content">
        <div class="mobile-title">
          Для ценителей комфорта<br>и качества<br><span>в уходе за волосами</span>
        </div>
        
        <div class="mobile-image-block">
          <img src="@/assets/image/bg-slide-3.png" alt="motor" class="mobile-motor-img" />
          <div class="mobile-motor-text">Бесщеточный<br>мотор BLDC</div>
        </div>

        <div class="mobile-power-circle">
          <img src="@/assets/image/coffee-star.svg" alt="power" class="coffee-img" />
          <span class="power-text">Салонная<br>мощность</span>
        </div>
      </div>

    </div>
  </div>
</template>

<script setup>
import { computed, ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  slideIndex: {
    type: Number,
    required: true
  }
})

const isMobile = ref(false)

const checkMobile = () => {
  isMobile.value = window.innerWidth <= 540
}

onMounted(() => {
  checkMobile()
  window.addEventListener('resize', checkMobile)
})

onUnmounted(() => {
  window.removeEventListener('resize', checkMobile)
})

const bgImage = computed(() => {
  const index = props.slideIndex + 1
  if (isMobile.value) {
    return new URL(`../../assets/image/bg-slider${index}-mob.png`, import.meta.url).href
  }
  return new URL(`../../assets/image/bg-slide-${index}.png`, import.meta.url).href
})
</script>

<style scoped>
.yamaguchi-slide {
  width: 100%;
  height: 100%;
  border-radius: 70px;
  padding: 50px;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  position: relative;
}

.slide-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  height: 100%;
}

.title {
  font-size: 47px;
  line-height: 1;
  color: #4B4B4B;
  text-align: center;
}

.badge {
  margin-top: 40px;
  width: 161px;
  height: 74px;
  background-color: #ffffff;
  font-size: 53px;
  border-radius: 50px;
  color: #4B4B4B;
  display: flex;
  justify-content: center;
  align-items: center;
}

.side-blocks {
  position: absolute;
  bottom: 40px;
  left: 50px;
  right: 50px;
  display: flex;
  justify-content: space-between;
  pointer-events: none;
}

.side-item {
  display: flex;
  flex-direction: column;
}

.side-item.left {
  align-items: flex-start;
}

.side-item.right {
  align-items: flex-end;
}

.side-label {
  height: 55px;
  padding: 0 27px;
  border-radius: 33px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 27px;
  margin-bottom: 20px;
}

.label-dark {
  background-color: #4B4B4B;
  color: #FFFFFF;
}

.label-light {
  background-color: #FFFFFF;
  color: #4B4B4B;
}

.side-image {
  width: 100%;
  max-width: 250px;
  height: 431px;
  border-radius: 32px;
  object-fit: cover;
}

.second-slide {
  position: relative;
  justify-content: center;
  align-items: flex-end;
  padding-right: 100px;
}

.coffee-circle {
  position: absolute;
  top: 0;
  left: 0;
  width: 180px;
  height: 180px;
  display: flex;
  align-items: center;
  justify-content: center;
  transform: rotate(-10deg);
}

.coffee-img {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
}

.coffee-text {
  position: relative;
  z-index: 2;
  font-size: 21px;
  color: #FFFFFF;
  text-align: center;
  font-weight: 600;
}

.second-right {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.title-second {
  font-size: 60px;
  line-height: 1;
  color: #FFFFFF;
  margin-bottom: 34px;
  text-transform: uppercase;
}

.description {
  font-family: 'Montserrat', sans-serif;
  font-weight: 500;
  font-size: 15px;
  color: #FFFFFF;
  margin-bottom: 39px;
  line-height: 1.5;
  text-align: center;
}

.badge-second {
  width: 126px;
  height: 55px;
  background-color: #ffffff;
  font-size: 27px;
  border-radius: 50px;
  color: #4B4B4B;
  display: flex;
  justify-content: center;
  align-items: center;
}

.third-slide {
  position: relative;
  justify-content: flex-start;
  align-items: flex-end;
}

.third-text-block {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  text-align: right;
  margin-top: 10px;
}

.title-third {
  font-size: 45px;
  line-height: 1.1;
  color: #4B4B4B;
  margin-bottom: 14px;
  text-transform: uppercase;
}

.subtitle-third {
  font-size: 34px;
  color: #737373;
}

.power-circle {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 160px;
  height: 160px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.power-text {
  position: relative;
  z-index: 2;
  font-size: 18px;
  color: #FFFFFF;
  text-align: center;
  font-weight: 600;
}

.mobile-third-content {
  display: none;
}

@media (max-width: 540px) {
  .yamaguchi-slide {
    border-radius: 0;
    padding: 20px;
    height: 100vh;
  }

  .slide-1 {
    background-position: center 80%;
  }

  .slide-2 {
    background-color: #7F7875;
    justify-content: flex-start;
    align-items: center;
    text-align: center;
    padding-top: 10px;
  }

  .title {
    font-size: 24px;
    margin-top: 10px;
  }

  .badge {
    margin-top: 15px;
    width: 90px;
    height: 45px;
    font-size: 28px;
  }

  .side-blocks {
    position: static;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
    width: 100%;
    margin-top: auto;
    margin-bottom: 120px;
  }

  .side-item {
    width: 100%;
    position: relative;
    border-radius: 32px;
    overflow: hidden;
    display: flex;
    justify-content: center;
  }

  .side-item.left,
  .side-item.right {
    align-items: center;
    text-align: center;
  }

  .side-label {
    position: absolute;
    top: 15px;
    z-index: 10;
    margin-bottom: 0;
    font-size: 16px;
    height: 35px;
    padding: 0 15px;
  }

  .side-item.left .side-label {
    left: 50%;
    transform: translateX(-50%);
  }

  .side-item.right .side-label {
    left: 50%;
    transform: translateX(-50%);
  }

  .side-image {
    width: 100%;
    max-width: 100%;
    height: auto;
    border-radius: 32px;
    display: block;
  }

  .side-item.right .side-image {
    transform: translateX(0);
  }

  .second-slide {
    align-items: center;
    padding-right: 0;
    justify-content: flex-start;
    padding-top: 20px;
  }

  .coffee-circle {
    position: absolute;
    top: 150px;
    left: auto;
    right: 20px;
    width: 90px;
    height: 90px;
    transform: rotate(-10deg);
  }

  .coffee-text {
    font-size: 12px;
    line-height: 1;
  }

  .title-second {
    font-size: 40px;
    margin-bottom: 15px;
    text-align: center;
  }

  .badge-second {
    width: 100px;
    height: 45px;
    font-size: 22px;
    margin-bottom: 0;
  }

  .second-right {
    width: 100%;
    align-items: center;
  }

  .description {
    position: absolute;
    bottom: 120px;
    font-size: 14px;
    margin-bottom: 0;
    width: 90%;
    text-align: center;
    color: #FFFFFF;
  }

  .third-slide {
    align-items: center;
    justify-content: flex-start;
    text-align: center;
    padding-top: 10px;
  }

  .third-text-block {
    display: none;
  }

  .power-circle {
    display: none;
  }

  .mobile-third-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
    position: relative;
    margin-top: 10px;
  }

  .mobile-title {
    font-size: 32px;
    line-height: 1.1;
    color: #FFFFFF;
    text-transform: uppercase;
    margin-bottom: 20px;
    font-weight: 600;
  }

  .mobile-title span {
    font-size: 22px;
  }

  .mobile-image-block {
    position: relative;
    width: 100%;
    height: 400px;
    border-radius: 30px;
    overflow: hidden;
  }

  .mobile-motor-img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
  }

  .mobile-motor-text {
    position: absolute;
    top: 20px;
    left: 20px;
    color: #4B4B4B;
    font-size: 18px;
    font-weight: 600;
    text-align: left;
    text-transform: uppercase;
  }

  .mobile-power-circle {
    position: absolute;
    bottom: -110px;
    left: 50%;
    transform: translateX(-50%);
    width: 180px;
    height: 180px;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 3;
  }
}
</style>