<template>
  <section>
    {{ tip }}
  </section>
</template>

<script>
import {
  inject 
} from 'vue'
export default {
  setup (props, context) {
    const tip = inject('tip')
    // const aa = () => {
    //   console.log('aaaa')
    // }
    return {
      // aa,
      tip
    }
  },
}
</script>

<style lang="scss">
</style>