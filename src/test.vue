<template>
  <section>
    <h2>{{ title }}</h2>
    <h3>{{ userInfo.userName }}</h3>
    <h4 @click="getUserCity">哈哈哈哈哈哈哈哈{{ city }}</h4>
    <h5 @click="set('999')">{{ userMsg }}</h5>
    <Child ref="childTest"></Child>
    <div>
      {{ text }}
      <input type="text" v-model="text">
    </div>
  </section>
</template>

<script>
import {
  reactive,
  ref,
  isRef,
  toRefs,
  computed,
  watch,
  readonly,
  customRef,
  onMounted
} from 'vue'
import Child from './child.vue'
export default {
  components: {
    Child
  },
  props: {
    title: {
      type: String,
      default: ''
    }
  },
  setup (props, { attrs, slots, parent, root, emit, refs }) {
    console.log(props.title)
    // console.log(context)

    // reactive() 函数接收一个普通对象，返回一个响应式的数据对象,支持Map、Set、WeakSet、WeakMap数据结构
    const userInfo = reactive({ userName: 'testetste' })

    // ref() 函数用来根据给定的值创建一个响应式的数据对象，ref() 函数调用的返回值是一个对象，这个对象上只包含一个 .value 属性
    const city = ref('')
    const getUserCity = () => {
      city.value += '测试测试'
    }

    // isRef() 用来判断某个值是否为 ref() 创建出来的对象。 isProxy、isReactive、isReadonly都大同小异
    console.log(isRef(city))

    // toRefs() 函数可以将 reactive() 创建出来的响应式对象，转换为普通的对象，就是可以將reactive多层对象类型的响应对象，转化为普通类型的响应数据
    console.log(toRefs(userInfo))

    // computed 计算属性
    const age = ref('666')
    const userMsg = computed(() => {
      return `我的年龄是:${age.value}`
    })
    const set = (num) => {
      if (age.value === '666') {
        age.value = num
      } else {
        age.value = '666'
      }
    }

    // watch
    const look = watch(
      () => age.value, // 监听对象
      (val, oVal) => { // 回调
        console.log(val, oVal)
      },
      {
        lazy: true // 是否触发深度监听
      }
    )
    // 支持响应多个对象
    // const stop = watch(
    //   [
    //     () => age.value,
    //     () => city.value,
    //   ],
    //   (
    //     [age, city],
    //     [prevage, prevcity],
    //   ) => {
    //     console.log("age:", age, "prevage", prevage)
    //     console.log("city:", city, "prevcity", prevcity)
    //   },
    //   {
    //     lazy: true // 是否触发深度监听
    //   }
    // )

    // ref 组件实例获取(需挂载到 return对象中，不然获取不到实例)
    const childTest = ref(null)
    onMounted(() => {
      console.log(childTest.value.tip)
    })

    // readonly 属性只读
    const readonlyTest = readonly({ count: 1 })
    readonlyTest.count++ // 数值不变，warn警告
    console.log(readonlyTest.count)

    // 创建一个可以控制其get set触发更新的引用对象，返回一个响应式的ref对象
    // 使用 customRef 实现防抖
    const useDebounce = (value, delay = 1000) => {
      return customRef((track, trigger) => {
        let timeout
        return {
          get () {
            track()//必须调用次函数才会触发更新
            return value
          },
          set (newValue) {
            clearTimeout(timeout)
            timeout = setTimeout(() => {
              value = newValue
              trigger()//必须调用次函数才会触发更新
            }, delay)
          }
        }
      })
    }
    return {
      userInfo,
      city,
      getUserCity,
      age,
      userMsg,
      set,
      look,
      childTest,
      text: useDebounce('测试啊')
    }
  },
}
</script>

<style lang="scss">
</style>