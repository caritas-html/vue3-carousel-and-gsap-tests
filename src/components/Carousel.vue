<template >
<div>
  
<div class="container">
   <!-- <carousel :wrap-around="true">  -->
    <!-- <slide v-for="slide in pokemons[0]" :key="slide"> -->

    <div class="module__wrapper carousel__item">
  <!-- <transition-group
    @enter="carouselLogic"
  > -->
      <ul v-for="pokemon in pokemons[0]" :key="pokemon.name" class="module__slider" ref="pokemons">
      
        <li :key="pokemon.name">
          {{pokemon.name}}
        </li>
      </ul>
  <!-- </transition-group> -->
    </div>
    <!-- </slide> -->
  <!-- </carousel>  -->
</div>
  <button @click="spin">
    Spin
  </button>
</div>
</template>

<script lang="ts">
import { defineComponent, reactive, onMounted, onBeforeMount, ref } from "vue";
import { Carousel, Slide } from "vue3-carousel";
import { gsap } from 'gsap';

import 'vue3-carousel/dist/carousel.css';


export default defineComponent({
  name: "carousel",
  components: {
    // Carousel,
    // Slide,
  },
  setup() {
    let pokemons: any = reactive([]) 
    let arr: string[] = []
    
    const sizeList = (): string[] => {
      let number = 0
      for (let i = 0; i < 16; i++) {
        number = number - 295
        arr.push(number + 'px')
      }
      console.log(arr)
      return arr
    } 

    const fetchApi = async () => {
      await fetch("https://pokeapi.co/api/v2/pokemon", {
        method: 'GET',
        headers: { 'Content-Type': 'application/json' },
      })
      .then(res => res.json())
      .then(res => res.results.slice(0, 15))
      .then(res => pokemons.push(res))
    }

    onBeforeMount(() => {
      sizeList()
    })

    onMounted(() => {
      fetchApi()
      })

    let rangeValue = gsap.utils.mapRange(15, 0, -4500, 0);
    let randomRangeValue = ref(Math.round(Math.random() * 15))

    // try to build a carousel logic //
    const carouselLogic = (el: any, done: any) =>
      gsap.to(el, {
      duration: 4,
      x: `${rangeValue(14)}px`,
      ease: 'elastic.out(1, 0.2)',
      delay: 0.3,
      onComplete: done,
     })

    return {
      pokemons,
      carouselLogic,
      rangeValue,
      randomRangeValue,
    }
  },
  methods: {
    spin() {
      gsap.to(".module__slider", {
        duration: 4,
        x: `${this.rangeValue(Math.round(Math.random() * 15))}px`,
        ease: 'elastic.out(1, 0.2)',
        delay: 0.3,
     })
    }
  }
})
</script>

<style>
  .container {
    display: flex;
    justify-content: flex-start;
    flex-direction: row;
    width: 4500px;
/* 9000px - 300px */
    z-index: -1;
  }

  .module__slider {
    width: fit-content;
    display: flex;
    justify-content: center;
    width: 295px;
    height: 300px;
    background: greenyellow;
    margin: 0 5px;
  }

  .module__wrapper {
    height: 350px;
    width: 4500px;
    flex-direction: row;
    display: flex;
    justify-content: center;
    overflow: hidden;
  }

  .pokemon {

  }

  ul {
    padding: 0;
  }
</style>