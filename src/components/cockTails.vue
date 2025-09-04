<template>
    <section id="cocktails" class="noisy">
        <img src="/images/cocktail-left-leaf.png" alt="l-leaf" id="c-left-leaf"/>
        <img src="/images/cocktail-right-leaf.png" alt="r-leaf" id="c-right-leaf"/>

        <div class="list">
            <div class="popular">
                <h2>Most popular cocktails:</h2>
                <ul>
                    <li v-for="item in cocktailLists" :key="item.name">
                        <div class="md:me-28">
                            <h3>{{ item.name }}</h3>
                            <p>{{ item.country }} | {{ item.detail }}</p>
                        </div>
                        <span>- {{ item.price }}</span>
                    </li>
                </ul>
            </div>

            <div class="loved">
                <h2>Most loved mocktails:</h2>
                <ul>
                    <li v-for="item in mocktailLists" :key="item.name">
                        <div class="me-28">
                            <h3>{{ item.name }}</h3>
                            <p>{{ item.country }} | {{ item.detail }}</p>
                        </div>
                        <span>- {{ item.price }}</span>
                    </li>
                </ul>
            </div>
        </div>
    </section>
</template>

<script setup>
import { onMounted } from 'vue';
import {cocktailLists} from '../constants/index.js'
import {mocktailLists} from '../constants/index.js'
import { ScrollTrigger } from 'gsap/all';
import gsap from 'gsap';
gsap.registerPlugin(ScrollTrigger);

onMounted(()=>{
    const parallaxTimeline=gsap.timeline({
        scrollTrigger:{
            trigger:'#cocktails',
            start:'top 30%',
            end:'bottom 80%',
            scrub:1,
        }
    });

    parallaxTimeline
    .from("#c-left-leaf",{x:-100,y:100})
    .from("#c-right-leaf",{x:100,y:100})
}

)
</script>

<style scoped>
</style>