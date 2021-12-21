<template>
  <div>
    <p> {{ counter }} </p>
    <button @click="increaseValue" > Increase </button>
  </div>
</template>

<script lang="ts">
import { defineComponent, watch, ref } from 'vue'

export default defineComponent({
  name: 'test-component',
  setup () {
    const counter = ref(0)

    const increaseValue = () => counter.value++
    // const counterEl = (() => h('p', counter.value))

    watch(counter, (newValue, oldValue) => {
      console.log(`The new value is ${newValue} and the old value is ${oldValue}`)
    })
    return {
      counter,
      increaseValue,
      // counterEl,
    }
  }
})
</script>

<style>

</style>