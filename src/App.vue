<template>
  <div class="app">
    <TestComponent />
    <Carousel/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import TestComponent from './components/TestComponent.vue';
import Carousel from './components/Carousel.vue';

export default defineComponent({
  name: 'App',
  components: {
    TestComponent,
    Carousel
}
});
</script>

<style lang="less">

</style>
