<template>
  <section>
    <Test :title="title"></Test>
  </section>
</template>

<script>
import Test from './test.vue'
// 生命周期
import {
  onBeforeMount,
  onMounted,
  onBeforeUpdate,
  onUpdated,
  onBeforeUnmount,
  onUnmounted,
  onErrorCaptured,
  onRenderTracked,
  onRenderTriggered,
  provide
} from 'vue'
export default {
  components: {
    Test
  },
  setup() {
    console.log('setup');
    onRenderTracked(() => {
      console.log('onRenderTracked');
    });
    onRenderTriggered((event) => {
      console.log('onRenderTriggered')
    });
    onBeforeMount(() => {
      console.log('onBeforeMount');
    });
    onMounted(() => {
      console.log('onMounted');
    });
    onBeforeUpdate(() => {
      console.log('onBeforeUpdate');
    });
    onUpdated(() => {
      console.log('onUpdated');
    });
    onBeforeUnmount(() => {
      console.log('beforeDestroy');
    });
    onUnmounted(() => {
      console.log('onUnmounted');
    });
    onErrorCaptured(() => {
      console.log('onErrorCaptured');
    });
    
    // provide & inject 共享普通数据,轻轻松松垮多级组件的数据传递
    // 父级组件中使用 provide() 函数向下传递数据；子级组件中使用 inject() 获取上层传递过来的数据
    provide('tip', '你好鸭')
    return {
      title: 'Hello World Vue3.0',
    };
  },
};
</script>

<style lang="scss">
body {
  padding: 50px;
  color: #42b983;
  text-align: center;
  background-color: #282c34;
}
</style>