<template>
    <main>
        <Navbar/>
        <Hero />
        <cockTails/>
        <!-- <div class="h-dvh bg-black"></div> -->
    </main>
    

</template>

<script setup>
import { onMounted } from 'vue'
import { ScrollTrigger ,SplitText } from 'gsap/all';


import Navbar from './components/Navbar.vue'
import Hero from './components/Hero.vue'
import cockTails from './components/cockTails.vue';


</script>

<style scoped>
</style>