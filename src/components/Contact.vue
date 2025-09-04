<template>
    <footer id="contact">
        <img src="/images/footer-left-leaf.png" alt="leaf-left" id="f-left-leaf" />
        <img src="/images/footer-right-leaf.png" alt="leaf-right" id="f-right-leaf" />
	 
     <div class="content">
        <h2>Where to Find Us</h2>

        <div>
            <h3>Visit Our Bar</h3>
            <p>同济大学嘉定校区友园20号楼</p>
        </div>

        <div>
            <h3>Contact Us</h3>
            <p>2353113@tongji.edu.cn</p>
        </div>

        <div>
            <h3>Open At No Time,no matter when</h3>
            <ul v-for="hour in openingHours" :key="hour.day">
                <li>{{ hour.day }} : {{ hour.time }}</li>
            </ul>
        </div>

        <div class="flex-center gap-5">
            <ul v-for="social in socials" :key="social.name">
                <a :href="social.url" :aria-label="social.name">
                    <img :src="social.icon">
                </a>
            </ul>
        </div>
     </div>
    </footer>
</template>

<script setup>
import {openingHours} from '../constants/index.js';
import {socials} from '../constants/index.js';
import { ScrollTrigger } from 'gsap/all';
import gsap from 'gsap';
import { onMounted } from 'vue';
gsap.registerPlugin(ScrollTrigger);

onMounted(()=>{
    const parallaxTimeline=gsap.timeline({
        scrollTrigger:{
            trigger:'#contact',
            start:'top 80%',
            end:'bottom 20%',
            scrub:1,
            markers: true
        }
    });

    parallaxTimeline
    .from("#f-left-leaf",{x:-100,y:100})
    .from("#f-right-leaf",{x:100,y:100})
})
</script>

