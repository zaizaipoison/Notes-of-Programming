### [简介 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/introduction.html)

### 项目构建

```
$ npm create vue@latest


$ cd <your-project-name>
$ npm install
$ npm run dev
```

下面命令会在 `./dist` 文件夹中为你的应用创建一个生产环境的构建版本。

```
npm run build
```

### 基础

ctrl+/加注释和去注释，可以直接按了之后写

### 组件传递数据

首先import到父组件，在父组件的html用子组件的名字创建

子组件需要设置好从父组件获得的属性，比如

let props = defineProps(['classify','sid','id']);

就是从父组件获得这三个属性，然后

let classify=ref(props.classify);

let sid=ref(props.sid);

let id=ref(props.id);

就取得了这三个属性

在父组件中，则需要用冒号传递所需要的属性，比如

```html
<historyDataDialog :classify="classify" :sid="sid" :id="id"/>
```

这里需要注意的是如果传的是直接与vmodel绑定的data，并且想要在父组件中及时更改，则要么在父组件中只通过调用函数改动内容，要么在子组件中不用let重新定义一个data，直接用data就好，因为props实际已经取出了data

这里有一个双向绑定的方法，父组件不变，子组件用下面这个方法

```typescript
let props = defineProps({
    classify:String,
    sid:String,
    id:String
});
let {classify,sid,id}=toRefs(props)
```

就可以获取到数值，并且是绑定的，前面那个方法是不可再变的

#### 父组件调子组件方法

先在子组件定义方法，并且主动将其暴露

```typescript
const childMethod = () => { console.log('child method.') } 
// 主动暴露childMethod方法
defineExpose({ childMethod })
```

在父组件中，定义子组件的ref，附加到子组件的html上，就可以通过ref调用其方法

```typescript
<child ref="childRef"></child>

const childRef = ref()
const callChildMethod = () => {
  childRef.value.childMethod()
}
```

### nextTick()

这个的用处是在下一次dom更新后获取最新信息，换句话说，如果你的更新是一次次的，那么这个就没什么用。

### Promise

这个是类似锁的用处,首先在用到这个的方法里要加上async,以声名这是个异步函数,然后,新建一个promises数组,在要等待完成的代码部分返回一个promise,最后,调用all,等待所有promise完成后,才会执行all里面的命令,我的理解是类似于你调用一个返回promise的函数时就给了一个锁,可以用于loading

```typescript
// 创建一个空数组来存储所有的 Promise
  const promises = [];        
  const promise = baseService.get("/operationMonitor/history/getClassInfo", {
          classify: item.class,
          sid: item.sid
        }).then((result) => {
          // 根据结果更新 tableData1, tableData2, tableData3
          updateTableData(result.data, item.class);
        });
        promises.push(promise);
// 使用 Promise.all 等待所有的请求完成
    Promise.all(promises).then(() => {
      // 所有请求完成之后才设置 loading 为 false
      state.loading = false;
    });
```

### json与字符串互转

JSON.parse()字符串转json（注意格式，key要有冒号的）

JSON.stringify()json转字符串

### ref和reactive的区别

[ref和reactive你必须要知道的使用场景和差异 - 前端专栏 - SegmentFault 思否](https://segmentfault.com/a/1190000043356331)

### 计算属性与方法

```javascript
let x=2
const func1=()=>{
  console.log("方法")
  return x
}
const com1=computed(()=>{
  console.log("计算属性")
  return x
})
```

上面的两个就分别是计算属性与方法，

*computed() 方法期望接收一个 getter 函数，返回值为一个计算属性 ref。和其他一般的 ref 类似，你可以通过 com1.value 访问计算结果。计算属性 ref 也会在模板中自动解包，因此在模板表达式中引用时无需添加 .value。*

*Vue 的计算属性会自动追踪响应式依赖。它会检测到 `com1` 依赖于 `x，所以当 `x` 改变时，任何依赖于 `com1` 的绑定都会同时更新。*

也就是说，计算属性实际是一个属性，他在自己所依赖的响应式变量改变时，才会重新计算，在被调用时，也只会访问前一次计算后存在缓存里的值，不会再走一遍运行逻辑，而函数则每次调用都会重新执行一次运行逻辑，所以计算属性的开销小很多

**一个计算属性的声明中描述的是如何根据其他值派生一个值，因此 getter 的职责应该仅为计算和返回该值，不要改变其他状态、在 getter 中做异步请求或者更改 DOM**

### v-if和v-show区别

v-if当条件为false时, 不会有对应的元素在DOM中
v-show当条件为false时, 仅仅是将元素的display属性设置为none而已

当需要在显示与隐藏之间切换很频繁时, 使用v-show  
当只有一次切换时, 使用 v-if
