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

```子组件
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

```子组件
const childMethod = () => { console.log('child method.') } 
// 主动暴露childMethod方法
defineExpose({ childMethod })
```

在父组件中，定义子组件的ref，附加到子组件的html上，就可以通过ref调用其方法

```父组件
<child ref="childRef"></child>

const childRef = ref()
const callChildMethod = () => {
  childRef.value.childMethod()
}
```


