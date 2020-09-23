---
title: '[Vue略懂系列]-组件通讯及插槽'
date: 2020-07-30 14:25:19
cover: /images/bg5.jpg
tags:
  - 前端
  - vue
categories:
  - 技术  
---

# 组件通讯

　　要用vue，就不得不提起它得组件化，万物皆组件，组件多了就不可避免的要串串门聊聊天啥的，这不串不要紧啊，这一串门，就出了大问题，到爷爷家该怎么打招呼呀，到兄弟家该怎么唠唠嗑，父子，兄弟，祖孙，曾曾祖孙，该守得规矩还得守，顿时是一个头变得两个大。

　　但是，作为一个打小品(lan)学(de)兼(dong)优(nao)的三好学生,怎么会被这点困难所打到，来个12345，安排的明明白白:

## 常用方法

### 1.父子传值

```
// child
props: { msg: String }
// parent
<Children msg="Hollo vue"/>
```
### 2.子给父传值

```
// child
this.$emit('add', good)
// parent
<Children @add="cartAdd($event)"></Children>
```
### 3.任意两组件传值

这里有两个常用的方法，
#### 方法1：事件总线
适用于比较简单的项目
```
// main.js
Vue.prototype.bus = new Vue()
// child1
this.$bus.$on('foo', handle) 
// child2
this.$bus.$emit('foo')

```

#### 方法2：vuex

创建唯一的全局数据管理者store，通过它管理数据并通知组件状态变更。

　　常用的到这里就结束了，但是，我们怎么能就止于此呢，既然要略懂略懂，那么不得在懂点什么吗，比如接下来的：

## 其他方法
### 4.兄弟组件通讯 $parent/$root

兄弟组件之间通信可通过共同祖辈搭桥，$parent或$root。
```
// brother1
this.$parent.$on('foo', handle) 
// brother2
this.$parent.$emit('foo')

```
### 5.父子通信 $children
父组件可以通过$children访问子组件实现父子通信。
```
// parent
this.$children[0].xx = 'xxx'
```
*[注释]：$children不能保证子元素顺序

### 6.$attrs/$listeners
包含了父作用域中不作为 prop 被识别 (且获取) 的特性绑定 (class和style除外)。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 (class和style除外)，并且可以通过v-bind="$attrs"传入内部组件——在创建高级别的组件时非常有用。
```
// child：并未在props中声明foo
<p>{{$attrs.foo}}</p>

// parent
<HelloWorld foo="foo"/>
```
### 7.获取子节点引用 refs
```
// parent
<HelloWorld ref="hw"/>

mounted() {  
  this.$refs.hw.xx = 'xxx'
}

```
### 8.祖先和后代之间传值 provide/inject
```
// ancestor
provide() {
  return {foo: 'foo'
}}

// descendant
inject: ['foo']
```

# 插槽

　　插槽语法是Vue 实现的内容分发 API，用于复合组件开发。该技术在通用组件库开发中有大量应用。

### 匿名插槽
```
// comp1
<div>  
  <slot></slot>
</div>

// parent
<comp>hello</comp>

```
### 具名插槽

将内容分发到子组件指定位置
```
// comp2
<div>  
  <slot></slot>  
  <slot name="content"></slot>
</div>

// parent
<Comp2>
  <!-- 默认插槽用default做参数 -->
  <template v-slot:default>具名插槽</template>
  <!-- 具名插槽用插槽名做参数 -->
  <template v-slot:content>内容...</template>
</Comp2>
```

### 作用域插槽

分发内容要用到子组件中的数据
```
// comp3
<div>  
  <slot :foo="foo"></slot>
</div>
// parent
<Comp3>
  <!-- 把v-slot的值指定为作用域上下文对象 -->
  <template v-slot:default="slotProps">来自子组件数据：{{slotProps.foo}}</template>
</Comp3>

```

　　组件通讯方式及插槽的一些简单介绍就到此结束,方便自己之后的复盘与学习查看, skr~