<template>
    <div id="art" ref="main">
      <div class="container mx-auto h-full pt-20">
        <h2 class="will-fade">The ART</h2>
        
        <div class="content">
          <ul class="space-y-4 will-fade">
            <li v-for="(feature, index) in goodLists" :key="`good-${index}`" class="flex items-center gap-2">
              <img src="/images/check.png" alt="check" />
              <p>{{ feature }}</p>
            </li>
          </ul>
          
          <div class="cocktail-img">
            <img
              src="/images/under-img.jpg"
              alt="cocktail"
              class="abs-center masked-img size-full object-contain"
            />
          </div>
          
          <ul class="space-y-4 will-fade">
            <li v-for="(feature, index) in featureLists" :key="`feature-${index}`" class="flex items-center justify-start gap-2">
              <img src="/images/check.png" alt="check" />
              <p class="md:w-fit w-60">{{ feature }}</p>
            </li>
          </ul>
        </div>
        
        <div class="masked-container">
          <h2 class="will-fade">Sip-Worthy Perfection</h2>
  
          <div id="masked-content">
            <h3>Made with Craft, Poured with Passion</h3>
            <p>This isn’t just a drink. It’s a carefully crafted moment made just for you.</p>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted, onUnmounted } from 'vue';
  import gsap from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  import { featureLists, goodLists } from '../constants/index.js'; // 确保路径正确
  
  // 注册 GSAP 插件
  gsap.registerPlugin(ScrollTrigger);
  
  const main = ref(null); // 组件根元素的引用
  let ctx; // GSAP Context 实例，用于安全地清理动画
  
  // 响应式逻辑：判断是否为移动端
  const mediaQuery = window.matchMedia('(max-width: 767px)');
  const isMobile = ref(mediaQuery.matches);
  const handleResize = () => { isMobile.value = mediaQuery.matches; };
  
  // GSAP 动画设置
  onMounted(() => {
    window.addEventListener('resize', handleResize);
  
    ctx = gsap.context(() => {
      // 动态设置 ScrollTrigger 的起始位置，以适应不同设备
      const start = () => isMobile.value ? 'top 20%' : 'top top';
  
      const maskTimeline = gsap.timeline({
        scrollTrigger: {
          trigger: '#art',
          start: start,
          end: 'bottom center',
          scrub: 1.5,
          pin: true,
        }
      });
  
      // 动画序列与原始 React 代码完全一致
      maskTimeline
        .to('.will-fade', { 
          opacity: 0, 
          stagger: 0.2, 
          ease: 'power1.inOut' 
        })
        .to('.masked-img', { 
          scale: 1.3,
          // index.css 中的 @utility masked-img 提供了初始遮罩样式
          // GSAP 在此基础上进行动画
          maskPosition: 'center', 
          maskSize: '400%', 
          duration: 1, 
          ease: 'power1.inOut' 
        })
        .to('#masked-content', { 
          opacity: 1, 
          duration: 1, 
          ease: 'power1.inOut'
        });
  
    }, main.value);
  });
  
  onUnmounted(() => {
    window.removeEventListener('resize', handleResize);
    // 组件销毁时，安全地清理所有 GSAP 动画和事件监听器
    if (ctx) {
      ctx.revert();
    }
  });
  </script>