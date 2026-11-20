<template>
  <div 
    class="yamaguchi-slider" 
    @wheel.prevent="onWheel"
    @touchstart="onTouchStart"
    @touchend="onTouchEnd"
  >
    <YamaguchiPagination
      :current-index="currentIndex"
      @change-slide="goToSlide"
      class="desktop-pagination"
    />

    <div
      v-for="(slide, index) in slides"
      :key="slide.id"
      class="slider-item"
      :class="getSlideClass(index)"
    >
      <div class="slide-wrapper">
        <YamaguchiSlide :slide-index="index" />
      </div>
    </div>

    <div class="mobile-bottom-nav">
      <div class="mobile-nav-content">
        <img :src="getActiveIcon()" class="mobile-nav-icon" />
        <span class="mobile-nav-text" v-html="getActiveText()"></span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { slides } from '@/data/slides'
import YamaguchiSlide from './YamaguchiSlide.vue'
import YamaguchiPagination from './YamaguchiPagination.vue'

const currentIndex = ref(0)
let isAnimating = false
let touchStartY = 0

const navData = [
  { icon: 'icon1-mob.svg', text: 'ЛЁГКОСТЬ<br>И КОМФОРТ В РУКЕ' },
  { icon: 'icon2-mob.svg', text: 'СКЛАДНАЯ<br>КОНСТРУКЦИЯ<br>FOLD ME' },
  { icon: 'icon3-mob.svg', text: 'ИННОВАЦИОННЫЙ<br>ЯПОНСКИЙ МОТОР' }
]

function getActiveIcon() {
  return new URL(`../../assets/image/${navData[currentIndex.value].icon}`, import.meta.url).href
}

function getActiveText() {
  return navData[currentIndex.value].text
}

function getSlideClass(index) {
  if (index === currentIndex.value) return 'is-active'
  if (index < currentIndex.value) return 'is-prev'
  return 'is-next'
}

function goToSlide(index) {
  if (isAnimating || index === currentIndex.value) return
  isAnimating = true
  currentIndex.value = index
  setTimeout(() => (isAnimating = false), 800)
}

function onWheel(event) {
  if (isAnimating) return
  if (event.deltaY > 0 && currentIndex.value < slides.length - 1) {
    isAnimating = true
    currentIndex.value++
    setTimeout(() => (isAnimating = false), 800)
  } else if (event.deltaY < 0 && currentIndex.value > 0) {
    isAnimating = true
    currentIndex.value--
    setTimeout(() => (isAnimating = false), 800)
  }
}

function onTouchStart(e) {
  touchStartY = e.changedTouches[0].screenY
}

function onTouchEnd(e) {
  const touchEndY = e.changedTouches[0].screenY
  const diff = touchStartY - touchEndY

  if (Math.abs(diff) > 50) {
    if (diff > 0 && currentIndex.value < slides.length - 1) {
      goToSlide(currentIndex.value + 1)
    } else if (diff < 0 && currentIndex.value > 0) {
      goToSlide(currentIndex.value - 1)
    }
  }
}
</script>

<style scoped>
.yamaguchi-slider {
  width: 100%;
  height: 100vh;
  background-color: #7F7875;
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.slider-item {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.8s cubic-bezier(0.4, 0, 0.2, 1),
              opacity 0.8s ease;
}

.slider-item.is-active {
  transform: translateY(0);
  opacity: 1;
  z-index: 3;
}

.slider-item.is-prev {
  transform: translateY(-100%);
  opacity: 0;
  z-index: 1;
}

.slider-item.is-next {
  transform: translateY(100%);
  opacity: 0;
  z-index: 2;
}

.slide-wrapper {
  width: 100%;
  max-width: 1240px;
  height: 100%;
  max-height: 800px;
  padding: 20px;
}

.mobile-bottom-nav {
  display: none;
}

@media (max-width: 540px) {
  .desktop-pagination {
    display: none;
  }

  .slide-wrapper {
    max-width: 100%;
    max-height: 100%;
    padding: 0;
    display: flex;
    justify-content: center;
  }

  .mobile-bottom-nav {
    display: flex;
    position: absolute;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    width: calc(100% - 30px);
    height: 100px;
    background-color: rgba(237, 232, 228, 0.9);
    border-radius: 60px;
    z-index: 50;
    align-items: center;
    padding: 14px;
  }

  .mobile-nav-content {
    display: flex;
    align-items: center;
    gap: 15px;
  }

  .mobile-nav-icon {
    width: 76px;
    height: 76px;
    object-fit: contain;
  }

  .mobile-nav-text {
    font-size: 21px;
    color: #4B4B4B;
    line-height: 1.1;
    text-transform: uppercase;
    font-weight: 600;
    text-align: left;
  }
}
</style>