<template>
  <div class="pagination-container">
    <div class="pagination-track"></div>

    <button
      v-for="(slide, index) in slides"
      :key="slide.id"
      class="pagination-btn"
      :style="getButtonStyle(index)"
      @click="$emit('change-slide', index)"
    >
      <img
        :src="getIconPath(index, index === currentIndex)"
        alt="icon"
        :class="['pagination-icon', { 'is-active': index === currentIndex }]"
      />
      <transition name="fade-slide">
        <div v-if="index === currentIndex" class="pagination-text">
          <template v-if="index === 0">ФЕН-СТАЙЛЕР<br>2 В 1</template>
          <template v-else-if="index === 1">СУПЕР-<br>ЛЕГКИЙ</template>
          <template v-else-if="index === 2">ИННОВАЦИОННЫЙ<br>ЯПОНСКИЙ МОТОР</template>
        </div>
      </transition>
    </button>
  </div>
</template>

<script setup>
import { slides } from '@/data/slides'

const props = defineProps({
  currentIndex: {
    type: Number,
    required: true
  }
})

defineEmits(['change-slide'])

const iconMap = [
  { normal: 'hairdryer.svg', active: 'hairdryer-white.svg' },
  { normal: 'weight.svg', active: 'weight-white.svg' },
  { normal: 'motor.svg', active: 'motor-white.svg' }
]

function getIconPath(index, isActive) {
  const icons = iconMap[index] || iconMap[0]
  const fileName = isActive ? icons.active : icons.normal
  return new URL(`../../assets/image/${fileName}`, import.meta.url).href
}

function getButtonStyle(index) {
  const diff = index - props.currentIndex
  
  const radius = 400
  const angleStep = 12
  const centerX = -320
  const centerY = 0
  
  const angle = diff * angleStep
  const radian = (angle * Math.PI) / 180
  
  const x = Math.cos(radian) * radius + centerX
  const y = Math.sin(radian) * radius + centerY

  return {
    top: '50%',
    transform: `translate(${x}px, calc(-50% + ${y}px))`,
    zIndex: diff === 0 ? 10 : 5
  }
}
</script>

<style scoped>
.pagination-container {
  position: absolute;
  left: 50px;
  top: 0;
  width: 150px;
  height: 100vh;
  z-index: 10;
  pointer-events: none;
}

.pagination-track {
  position: absolute;
  left: -50px;
  top: 0;
  width: 150px;
  height: 100%;
  background-image: url('@/assets/image/Ellipse.svg');
  background-size: 100% 100%;
  background-position: left center;
  background-repeat: no-repeat;
  z-index: 1;
  pointer-events: none;
}

.pagination-btn {
  position: absolute;
  left: 0;
  background: transparent;
  border: none;
  padding: 0;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
  pointer-events: auto;
}

.pagination-icon {
  transition: all 0.6s cubic-bezier(0.4, 0, 0.2, 1);
  object-fit: contain;
  width: 74px;
  height: 74px;
  flex-shrink: 0;
  margin-left: -12px;
}

.pagination-icon.is-active {
  width: 85px;
  height: 85px;
  opacity: 1;
}

.pagination-text {
  position: absolute;
  left: 70px;
  top: 7px;
  width: max-content;
  text-align: left;
  font-family: 'Euclid Circular A';
  font-weight: 600;
  font-style: normal;
  font-size: 20px;
  line-height: 28px;
  letter-spacing: -0.01em;
  text-transform: uppercase;
  color: #EDE8E4;
  pointer-events: none;
}

.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: opacity 0.4s ease, transform 0.4s ease;
}

.fade-slide-enter-from,
.fade-slide-leave-to {
  opacity: 0;
  transform: translateX(-20px);
}
</style>