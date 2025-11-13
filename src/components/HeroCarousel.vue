

<template>
  <section
    class="hero"
    @mouseenter="setPauseState(true)"
    @mouseleave="setPauseState(false)"
  >
    <div class="hero__slides">
      <div
        v-for="(slide, index) in slides"
        :key="slide.id"
        class="hero__slide"
        :class="{ 'hero__slide--active': index === currentIndex }"
        :style="{ backgroundImage: `url(${slide.image})` }"
      >
        <div class="hero__overlay"></div>
      </div>
    </div>

    <div class="hero__content">
      <transition name="text-slide" mode="out-in">
        <div :key="textTransitionKey" class="hero__copy">
          <p class="hero__label">
            <span class="hero__label-accent">{{ currentSlide?.label }}</span>
          </p>
          <h1 class="hero__headline">{{ currentSlide?.headline }}</h1>
          <p class="hero__description">
            {{ currentSlide?.description }}
          </p>
          <div class="hero__stats-divider"></div>
          <ul class="hero__stats">
            <li v-for="item in currentSlide?.stats ?? []" :key="item.note">
              <span class="hero__stat-value">{{ item.value }}</span>
              <span class="hero__stat-note">{{ item.note }}</span>
            </li>
          </ul>
          <button class="hero__cta">Explore Services</button>
        </div>
      </transition>
    </div>

    <button
      class="hero__nav hero__nav--left"
      @click="goToPrev"
      aria-label="Previous slide"
    >
      <span>&lsaquo;</span>
    </button>
    <button
      class="hero__nav hero__nav--right"
      @click="goToNext"
      aria-label="Next slide"
    >
      <span>&rsaquo;</span>
    </button>

    <div class="hero__indicators">
      <button
        v-for="(slide, index) in slides"
        :key="slide.id"
        class="hero__indicator"
        :class="{ 'hero__indicator--active': index === currentIndex }"
        @click="goToIndex(index)"
        :aria-label="`Go to slide ${index + 1}`"
      ></button>
    </div>
  </section>
</template>

<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, ref, watch } from "vue";



interface Slide {
  id: number;
  image: string;
  label: string;
  headline: string;
  description: string;
  stats: Array<{ value: string; note: string }>;
}

const slides = ref<Slide[]>([
  {
    id: 0,
    image:
      "https://images.unsplash.com/photo-1582719478250-c89cae4dc85b?auto=format&fit=crop&w=2000&q=80",
    label: "Genetic Innovation",
    headline: "Innovative seed on board",
    description:
      "Your trusted CRO partner for advanced gene editing and delivery solutions, accelerating research from discovery to therapy.",
    stats: [
      { value: "4.9/5", note: "Avg. rating" },
      { value: "300+", note: "Trusted partners" },
      { value: "99%", note: "Success rate" },
    ],
  },
  {
    id: 1,
    image:
      "https://images.unsplash.com/photo-1584036561566-baf8f5f1b144?auto=format&fit=crop&w=2000&q=80",
    label: "Therapy Delivery",
    headline: "Precision platform for viral vectors",
    description:
      "From discovery through clinical manufacturing, we deliver scalable vector solutions and transparent project coordination.",
    stats: [
      { value: "24/7", note: "Program visibility" },
      { value: "10+", note: "Regulatory approvals" },
      { value: "120", note: "Clinical programs" },
    ],
  },
  {
    id: 2,
    image:
      "https://images.unsplash.com/photo-1526256262350-7da7584cf5eb?auto=format&fit=crop&w=2000&q=80",
    label: "Partner Experience",
    headline: "Designed for collaboration",
    description:
      "Integrated analytics and quality insights that help you navigate milestones faster and improve therapeutic outcomes.",
    stats: [
      { value: "50%", note: "Faster onboarding" },
      { value: "2x", note: "Faster iteration cycles" },
      { value: "100%", note: "Regulatory compliance" },
    ],
  },
]);

const autoplayDelay = 7000;
const currentIndex = ref(0);
const textAnimationSeed = ref(0);
const isPausing = ref(false);
let timer: number | undefined;

const currentSlide = computed(() => slides.value[currentIndex.value]);

const stopAutoplay = () => {
  if (timer !== undefined) {
    clearInterval(timer);
    timer = undefined;
  }
};

const startAutoplay = () => {
  stopAutoplay();
  timer = window.setInterval(() => {
    if (!isPausing.value) {
      goToNext();
    }
  }, autoplayDelay);
};

const triggerTextAnimation = () => {
  textAnimationSeed.value++;
};

const goToIndex = (index: number) => {
  if (index === currentIndex.value) {
    return;
  }
  currentIndex.value = (index + slides.value.length) % slides.value.length;
  triggerTextAnimation();
};

const goToNext = () => {
  goToIndex(currentIndex.value + 1);
};

const goToPrev = () => {
  goToIndex(currentIndex.value - 1);
};

const setPauseState = (value: boolean) => {
  isPausing.value = value;
};

onMounted(() => {
  startAutoplay();
});

onBeforeUnmount(() => {
  stopAutoplay();
});

watch(
  () => slides.value.length,
  () => {
    goToIndex(0);
    startAutoplay();
  }
);

const textTransitionKey = computed(
  () => `${currentSlide.value?.id ?? "slide"}-${textAnimationSeed.value}`
);
</script>

<style scoped>
.hero {
  position: relative;
  width: 100%;
  height: 100vh;
  max-height: 720px;
  color: #0f172a;
  overflow: hidden;
  background-color: #f3f6fb;
}

.hero__slides {
  position: absolute;
  inset: 0;
}

.hero__slide {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  opacity: 0;
  transition: opacity 0.9s ease-in-out;
}

.hero__slide--active {
  opacity: 1;
}

.hero__overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    120deg,
    rgb(248, 250, 252) 0%,
    rgba(248, 250, 252, 0.815) 48%,
    rgba(248, 250, 252, 0) 100%
  );
}

.hero__content {
  position: relative;
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  padding: 0 5vw;
}

.hero__copy {
  max-width: 680px;
  width: 100%;
  margin: 0 auto;
  color: #0f172a;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  text-align: left;
}

.hero__label {
  letter-spacing: 0.08em;
  text-transform: uppercase;
  font-weight: 700;
  font-size: 1.05rem;
  color: #0ea5e9;
  margin-bottom: 0.15rem;
}

.hero__label-accent {
  display: inline-block;
  background: linear-gradient(90deg, #2563eb 0%, #22d3ee 100%);
  -webkit-background-clip: text;
  color: transparent;
}

.hero__headline {
  font-size: clamp(2.5rem, 3vw + 1.5rem, 3.8rem);
  font-weight: 700;
  line-height: 1.15;
  color: #0f172a;
  margin-bottom: 1rem;
  white-space: nowrap;
}

.hero__description {
  font-size: 1.05rem;
  line-height: 1.6;
  color: #475569;
  margin-bottom: 2rem;
}

.hero__stats {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 2.5rem;
  margin: 0;
  padding: 0;
  list-style: none;
}

.hero__stats li {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #64748b;
}

.hero__stat-value {
  font-size: 1.5rem;
  font-weight: 600;
  color: #0f172a;
}

.hero__stat-note {
  font-size: 0.95rem;
  margin-top: 0.25rem;
}

.hero__cta {
  margin-top: 2rem;
  padding: 0.85rem 1.8rem;
  border-radius: 9999px;
  border: none;
  background-color: #2563eb;
  color: white;
  font-weight: 600;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.hero__cta:hover {
  transform: translateY(-1px);
  box-shadow: 0px 15px 30px rgba(37, 99, 235, 0.35);
}

.hero__nav {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 44px;
  height: 44px;
  border-radius: 50%;
  border: none;
  background: rgba(255, 255, 255, 0.8);
  color: #0f172a;
  font-size: 2rem;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.3s ease, transform 0.3s ease;
  z-index: 3;
}

.hero__nav:hover {
  background: #616263;
  transform: translateY(-50%) scale(1.05);
  color: #ffffff;
}

.hero__nav--left {
  left: 24px;
}

.hero__nav--right {
  right: 24px;
}

.hero__indicators {
  position: absolute;
  left: 50%;
  bottom: 24px;
  transform: translateX(-50%);
  display: flex;
  gap: 12px;
  z-index: 3;
}

.hero__indicator {
  width: 12px;
  height: 12px;
  border-radius: 9999px;
  border: none;
  background: rgba(15, 23, 42, 0.2);
  cursor: pointer;
  transition: width 0.3s ease, background 0.3s ease;
}

.hero__indicator--active {
  width: 36px;
  background: #1d4ed8;
}

.text-slide-enter-active,
.text-slide-leave-active {
  transition: opacity 0.45s ease, transform 0.45s ease;
}

.text-slide-enter-from {
  opacity: 0;
  transform: translateX(36px);
}

.text-slide-enter-to {
  opacity: 1;
  transform: translateX(0);
}

.text-slide-leave-from {
  opacity: 1;
  transform: translateX(0);
}

.text-slide-leave-to {
  opacity: 0;
  transform: translateX(-36px);
}

@media (max-width: 960px) {
  .hero {
    max-height: none;
    height: auto;
    min-height: 80vh;
  }

  .hero__content {
    padding: 6rem 10vw 5rem;
  }

  .hero__stats {
    gap: 1.5rem;
  }
}

@media (max-width: 640px) {
  .hero__nav {
    display: none;
  }

  .hero__content {
    padding: 5rem 1.5rem 4rem;
  }

  .hero__description {
    font-size: 1rem;
  }

  .hero__stats {
    flex-direction: column;
    gap: 1rem;
  }

  .hero__indicator--active {
    width: 20px;
  }
}
</style>

