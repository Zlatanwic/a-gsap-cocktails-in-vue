<template>
    <section id="hero" class="noisy">
        <h1 class="title">VODKA</h1>
        <img src="/images/hero-left-leaf.png" alt="left-leaf" class="left-leaf"/>
        <img src="/images/hero-right-leaf.png" alt="right-leaf" class="right-leaf"/>

        <div class="body">
            <div class="content">
                <div class="space-y-5 hidden md:block">
                    <p>Cool. Crisp. Classic.</p>
                    <p class="subtitle">
                        Sip the Spirit <br/>
                        of Elegance
                    </p>
                </div>

                <div class="view-cocktails">
                    <p class="subtitle">
                        Every cocktail on our menu is a blend of premium ingredients,
				creative flair, and timeless recipes — designed to delight your
				senses.
                    </p>
                    <a href="#cocktails">View Cocktails</a>
                </div>
            </div>
        </div>
    </section>

    <div class="video absolute inset-0">
        <video ref="videoRef" src="/videos/output.mp4" muted playsinline preload="auto"></video>

        
    </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';

import { useMediaQuery } from '@vueuse/core';
import {SplitText,ScrollTrigger} from 'gsap/all';
import gsap from 'gsap';
gsap.registerPlugin(ScrollTrigger, SplitText);

const videoRef=ref(null);
const isMobile = useMediaQuery('(max-width: 767px)');

onMounted(()=>{
    const heroSplit=new SplitText('.title',{type:'chars,words'});

    const paragraphSplit=new SplitText('.subtitle',{type:'lines'});

    

    heroSplit.chars.forEach((char)=>char.classList.add('text-gradient'))
    gsap.from(heroSplit.chars,{
        yPercent:100,
        duration:1.8,
        ease:'expo.out',
        stagger:0.06,
    })
    gsap.from(paragraphSplit.lines,{
        yPercent:100,
        duration:1.5,
        ease:'expo.out',
        stagger:0.06,
        delay:1,}
    )

    gsap.timeline({
        scrollTrigger:{
            trigger:'#hero',
            start:'top top',
            end:'bottom top',
            scrub:true
        }
    })
    .to(".right-leaf", { y: 400 }, 0)
	.to(".left-leaf", { y: -400 }, 0)

    const startValue=isMobile.value?'top 50%':'center 60%';
    const endValue=isMobile.value?'120% top':'bottom top';

    if (videoRef.value) {
      
        const videoTl = gsap.timeline({
          scrollTrigger: {
            trigger: 'video', // 同样由 #hero 区域触发
            start: startValue,
            end: endValue,
            scrub: true,
            pin:true
            // markers: true // 开发时可以取消注释来调试
          }
        });
        videoRef.value.onloadedmetadata = () => {
        // 3. 动画的目标是视频的 currentTime 属性
        videoTl.to(videoRef.value, {
          currentTime: videoRef.value.duration
        });
      };
    }
}
)


</script>

<style scoped>
</style>