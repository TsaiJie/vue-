##  1、 vue基本配置

```javascript
//配置是否允许vue-devtools检查代码，方便调试，生产环境中需要设置为false.
Vue.config.devtools = false;
Vue.config.productionTip = false;
```

##  2、常用的指令

+ v-model 

  数据双向绑定，用于表单元素。

+ v-for  

  普通循环**“item in list”**和键值循环**“(item,index) in list”**，可以直接循环包含重复数据的集合，可以通过指定**:key**属性绑定唯一的key，使用索引或者id值。key的作用：当更新元素时可以重用元素，提高效率。

+ v-on:/ @  

  事件的绑定，用法：v-on:事件=“函数‘。

+ v-show/v-if 

  用来显示或隐藏元素，v-show是通过display实现，v-if每次删除后在重新创建。


## 3、事件和属性

### 1、事件

----



#### 1.1 事件简写

```javascript
v-on:click = "事件名称" =>  @click = "事件名称"
```



#### 1.2 事件对象$event

包含事件相关信息，如事件源、事件类型、偏移量target,type,offsetx,使用的时候$event作为参数传入。

```javascript
v-on:click = "print($event)";
```



#### 1.3 事件冒泡

阻止事件冒泡

a) 依赖js原生对象

```javascript
//html body中
v-on:click = "print($event)"
//vm实例方法中methods
print(e){
    e.stopPropagation();
}
```

b) vue方式

```javascript
v-on:click.stop = "print()";
```



#### 1.4 事件默认行为

a) 依赖js原生对象

```javascript
//html body中
v-on:click = "print($event)"
//vm实例方法中methods
print(e){
    e.preventDefault();
}
```

b) vue方式

```javascript
v-on:click.prevent = "print()";
```



#### 1.5 键盘事件

```javascript
回车： @keydown.enter/ @keydown.13
上： @keydown.up/@keydown.38
...

自定义键盘事件
Vue.config.keyCodes = {
    a:65,
}
```



#### 1.6 事件修饰符

- `.stop` - 调用 `event.stopPropagation()`。
- `.prevent` - 调用 `event.preventDefault()`。
- `.capture` - 添加事件侦听器时使用 capture 模式。
- `.self` - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
- `.{keyCode | keyAlias}` - 只当事件是从特定键触发时才触发回调。
- `.native` - 监听组件根元素的原生事件。
- `.once` - 只触发一次回调。
- `.left` - (2.2.0) 只当点击鼠标左键时触发。
- `.right` - (2.2.0) 只当点击鼠标右键时触发。
- `.middle` - (2.2.0) 只当点击鼠标中键时触发。
- `.passive` - (2.3.0) 以 `{ passive: true }` 模式添加侦听器



### 2、属性

----

#### 2.1 属性绑定和属性的简写

v-bind 用于属性的绑定，v-bind:属性 = “ ”，绑定之后可以访问vue实例中的数据。

```javascript
属性的简写：v-bind:src=" " =>  :src= " "
```

####  2.2 class和style属性



## 4、模板

### 1. 简介

Vue.js 使用了基于 HTML 的模板语法，可以将 DOM 绑定至底层 Vue 实例的数据。{{}}

### 2.数据绑定的方式

a. 双向绑定 v-model

b. 单向绑定 

 +  {{}}，可能会出现闪烁问题，可以用v-cloak解决
 +  v-text 、v-html

### 3.其他指令

v-once 数据只绑定一次

v-pre 不编译，直接原样显示



