<template>
    <div id="art">
        <div class="container mx-auto h-full pt-20">
            <h2 class="will-fade">The ART</h2>

            <div class="content">
                <ul class="space-y-4 will-fade">
                    <li v-for="item in goodLists" :key=item class="flex items-center gap-2">
                        <img src="/images/check.png" alt="check">
                        <p>{{ item }}</p>
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
                    <li v-for="item in featureLists" :key=item class="flex items-center justify-start gap-2">
                        <img src="/images/check.png" alt="check">
                        <p class="md:w-fit w-60">{{ item }}</p>
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
import {featureLists,goodLists} from '../constants/index.js'
import { ScrollTrigger } from 'gsap/all';
import { onMounted } from 'vue'
import { useMediaQuery } from '@vueuse/core';
import gsap from 'gsap';
gsap.registerPlugin(ScrollTrigger);
const isMobile=useMediaQuery('(max-width: 767px)');

onMounted(() => {
	const ctx = gsap.context(() => {
    const start = isMobile ? 'top 20%' : 'top top';
	
	const maskTimeline = gsap.timeline({
	 scrollTrigger: {
		trigger: '#art',
		start:'top 20%',
		end: 'bottom center',
		scrub: 1.5,
		pin: true,
		anticipatePin: 1
	 }
	})
	
	maskTimeline
	 .to('.will-fade', { opacity: 0, stagger: 0.2, ease: 'power1.inOut', })
	 .to('.masked-img', { scale: 1.3, maskPosition: 'center', maskSize: '400%', duration: 1, ease: 'power1.inOut '})
	 .to('#masked-content', { opacity: 1, duration: 1, ease: 'power1.inOut'})

	})

	// 4. 在组件卸载时，调用 revert() 清理所有在 context 中创建的动画
	onUnmounted(() => {
    ctx.revert();
  });
});

</script>

<style scoped>
</style>