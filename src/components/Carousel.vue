 <template>
    
  <Carousel
     :wrap-around="true"
     :itemsToShow="1.8"
     :mouse-drag="false" 
     :touch-drag="true" 
     :settings="settings">
    <Slide 
    v-for="pokemon in pokemons[0]" 
    :key="pokemon.name" 
    class="module__slider"
    >
        <div :key="pokemon.name" class="carousel__item">
          {{pokemon.name}}
        </div>
    </Slide>
    <template #addons="{ currentSlide }">
    <Navigation v-if="currentSlide" />
    </template>
  </Carousel>
  <button @click="spin">
    Spin
  </button>
</template>

<script lang="ts">
import { defineComponent, reactive, onMounted, ref } from "vue";
import { Carousel, Navigation, Slide } from "vue3-carousel";
import { gsap } from 'gsap';

import 'vue3-carousel/dist/carousel.css';


export default defineComponent({
  name: "carousel",
  components: {
    Carousel,
    Slide,
    Navigation,
  },
  setup() {
    let pokemons: any = reactive([]) 
    

    const fetchApi = async () => {
      await fetch("https://pokeapi.co/api/v2/pokemon", {
        method: 'GET',
        headers: { 'Content-Type': 'application/json' },
      })
      .then(res => res.json())
      .then(res => res.results.slice(0, 15))
      .then(res => pokemons.push(res))
    }

    onMounted(() => {
      fetchApi()
      })

    let rangeValue = gsap.utils.mapRange(0, 15, 0, -1675.33)
    let rangeValueTarget = gsap.utils.mapRange(0, -1675, 0, 15)
    let randomRangeValue = ref(Math.round(Math.random() * 15))

    // try to build a carousel logic //
    const carouselLogic = (el: any, done: any) =>
      gsap.to(el, {
      duration: 4,
      x: `${rangeValue(14)}px`,
      ease: 'bounce.out(1000)',
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
    numberOrder() {
      const nodes = this.$el.nextElementSibling.firstChild.firstChild.childNodes
      return nodes[Math.round(Math.random() * 14)].style.order
    },
     spin() {
      const nodes = this.$el.nextElementSibling.firstChild.firstChild.childNodes
      let nOrder = this.numberOrder()
      let orderActive;    
      
      gsap.from(".carousel__track", {
        x: `${this.rangeValue(Math.round(Math.random() * 14))}`,
        onComplete: () => {
          for (let i = 0; i < nodes.length; i++) {
            if (nodes[i].classList.contains("carousel__slide--active")) {
              nodes[i].style.order = 12;
              nodes[i].classList.remove("carousel__slide--prev")
            }
            if (nodes[i].classList.contains("carousel__slide--prev")) {
              nodes[i].style.order = parseInt(nOrder) - 1
            }
            if (nodes[i].classList.contains("carousel__slide--next")) {
              nodes[i].style.order = parseInt(nOrder) + 1
            }
          }
        }
      })

      gsap.to(".carousel__track", {
        duration: 4,
        x: function(index, target, targets) {
          console.log(targets)
          return targets[12]
        },
        ease: 'elastic.out(1, 0.2)',
        delay: 0.3,
      })
    //   gsap.to(".carousel__slide--prev", {
    //     rotation: -8,
    //     y: 27,
    //     x: -27,
    //   })

    //   gsap.to(".carousel__slide--next", {
    //     rotation: 8,
    //     y: 27,
    //     x: -27,
    //   })
      // let result = true
      
      // if (this.result) {
      //   console.log(!this.result)
      //   this.result = true
      //   return !this.result
      // } else {
      //   return true
      // }
    },
  },
  data: () => ({
    result: true,
    settings: {
     snapAlign: 'center',
     
    }
  })
})
</script>

<style>
  .module__slider {
    width: fit-content;
    display: flex;
    justify-content: center;
    width: 295px;
    height: 300px;
    background: greenyellow;
    margin: 0 5px;
  }

  .carousel__slide--visible {
    order: 12
  }

  .carousel__slide--prev {
    transform: rotate(352deg) translateY(27px) translateX(-27px);
  }

  .carousel__slide--next {
    transform: rotate(8deg) translateY(27px) translateX(27px);
  }

  .carousel__track {
    width: 356px;
  } 

  ul {
    padding: 0;
  }
</style>