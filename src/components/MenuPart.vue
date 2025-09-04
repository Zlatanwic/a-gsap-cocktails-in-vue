<template>
    <section id="menu" aria-labelledby="menu-heading">
        <img src="/images/slider-left-leaf.png" alt="left-leaf" id="m-left-leaf">
        <img src="/images/slider-right-leaf.png" alt="right-leaf" id="m-right-leaf">
        <h2 id="menu-heading" class="sr-only">Cocktail Menu</h2>
        <nav class="cocktail-tabs" aria-label="Cocktail Navigation">
            <button 
        v-for="(cocktail, index) in allCocktails"
        :key="cocktail.id"
        :class="index === currentIndex ? 'text-white border-white' : 'text-white/50 border-white/50'"
        @click="goToSlide(index)"
      >
        {{ cocktail.name }}
      </button>
        </nav>

        <div class="content">
      <div class="arrows">
        <button class="text-left" @click="goToSlide(currentIndex - 1)">
          <span>{{ prevCocktail.name }}</span>
          <img src="/images/right-arrow.png" alt="right-arrow" aria-hidden="true" />
        </button>
      
        <button class="text-left" @click="goToSlide(currentIndex + 1)">
          <span>{{ nextCocktail.name }}</span>
          <img src="/images/left-arrow.png" alt="left-arrow" aria-hidden="true" />
        </button>
      </div>
      
      <div class="cocktail">
        <img :src="currentCocktail.image" class="object-contain"/>
      </div>
      
      <div class="recipe">
        <div class="info">
          <p>Recipe for:</p>
          <p id="title">{{ currentCocktail.name }}</p>
        </div>
      
        <div class="details">
          <h2>{{ currentCocktail.title }}</h2>
          <p>{{ currentCocktail.description }}</p>
        </div>
      </div>
    </div>
    </section>
</template>

<script setup>
import { allCocktails } from '../constants/index.js';
import {ref,computed,watch,onMounted} from 'vue';
import gsap from 'gsap';

const currentIndex=ref(0);
const totalCocktails=allCocktails.length;


const goToSlide = (index) => {

  const newIndex = (index + totalCocktails) % totalCocktails;
  currentIndex.value = newIndex;
};

// 3. 派生状态 (Vue 的优势：Computed Properties)
// currentCocktail, prevCocktail, nextCocktail 在 React 中是每次渲染都重新计算的变量。
// 在 Vue 中，我们使用 `computed`，它会缓存结果，仅在依赖项 (currentIndex) 变化时才重新计算。
const currentCocktail = computed(() => {
  return allCocktails[currentIndex.value];
});

const prevCocktail = computed(() => {
  // 使用 (currentIndex.value - 1 + totalCocktails) % totalCocktails 来处理负数索引
  const prevIndex = (currentIndex.value - 1 + totalCocktails) % totalCocktails;
  return allCocktails[prevIndex];
});

const nextCocktail = computed(() => {
  const nextIndex = (currentIndex.value + 1) % totalCocktails;
  return allCocktails[nextIndex];
});

// 使用 watch 来监听 currentIndex 的变化，并触发动画
watch(currentIndex, () => {
  
  gsap.fromTo('.cocktail img', { opacity: 0, xPercent: -100 }, {
    xPercent: 0, opacity: 1, duration: 1, ease: 'power1.inOut'
  });
  gsap.fromTo('.details h2', { yPercent: 100, opacity: 0 }, {
    yPercent: 0, opacity: 1, ease: 'power1.inOut' 
  });
  gsap.fromTo('.details p', { yPercent: 100, opacity: 0 }, {
    yPercent: 0, opacity: 1, ease: 'power1.inOut'
  });
  gsap.fromTo('#title', { opacity: 0 }, { opacity: 1, duration: 1 });
});
</script>

<style scoped>
</style>