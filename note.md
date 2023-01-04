[TOC]

# 响应式
- 用于组合式API
- 对于setup中的数据
- 选项式API data中自动响应了
## reactive对于对象
```
let myData = Vue.reactive({
    value: 0
})
```
## ref对于独立值
- 自动包装对象
- 定义value属性为原始值
- 在返回中是独立值
```
let myData = Vue.ref(0);
myData.value
```
## toRefs解构
```
let myObject = Vue.reactive({
    value: 0
})
```
```
let {value1} = myObject
```
上面的value1已经不是响应式
```
let {value2} = Vue.toRefs(myObject)
```
上面的value2是响应式，被转换成ref对象变量
在setup中要使用value2.value

## 计算变量响应
```
let a = Vue.ref(1)
let b = Vue.ref(2)
let sum = Vue.computed(() => {
    return a.value + b.value
})
```
也支持赋值
> 在直接设置sum的值时调用set()
```
let sum = Vue.computed(() => {
    set(value) {
        a.value = value
        b.value = value
    },
    get() {
        return a.value + b.value
    }
})
```

## 监听变量响应
追踪watchEffect内部的变量，不用
```
let a = Vue.ref(1)
Vue.watchEffect(() => {
    console.log(a.value)
})
```
更好的watch方法
```
let a = Vue.reactive({data: 0})
let b = Vue.ref(0)
Vue.watch(() => {
    return a.data
}, (value, old) => {
    console.log(value, old)
})

Vue.watch(b, (value, old) => {
    console.log(value, old)
})
```
也支持同时监听多个
```
Vue.watch([() => {
    return a.data
}, b], ([valueA, valueB], [oldA, oldB]) => {
    ...
})
```
- 由`reactice()`包装的，就用`()=>{return a.data}`
- 由`ref()`直接赋值的，直接写
- 多个用列表包装
- 注意，后面新列表，旧列表顺序对应


# setup方法
- 不要使用this关键字