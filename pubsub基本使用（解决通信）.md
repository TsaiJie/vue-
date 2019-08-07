

## 1 pubsub-js解决数据通信

```
使用了如下几个插件
npm install axios -S
npm i pubsub-js -S
```

将header组件中的数据传递给兄弟组件main，实现兄弟间的数据通信，两者都挂载到APP组件下。

```vue
<template>
  <div class="header">
    <div class="container">
      <div>
        <h1>Search Github Users</h1>
        <input type="text" v-model="searchName" placeholder="请输入github用户名">
        <input type="button" value="Search"  @click="search">

      </div>
    </div>
  </div>
</template>


<script>
import PubSub from "pubsub-js"
export default {
  name:"Header",
  data(){
    return{
      searchName:""
    }
  },
  methods:{
    search(){
      // 发布消息，监听事件触发
      const search = this.searchName.trim();
      if(this.searchName){
        // 相当于发送数据
        PubSub.publish("search",this.searchName);
      }
    }
  }
}
</script>

<style>

</style>
```

```vue
<template>
  <div id="main">
    <h1 v-if="loading">loading</h1>
    <h1 v-if="errormsg">{{errormsg}}</h1>
    <div class="row">
      <div class="item" v-for="(user,index) in users" :key="index">
        <a :href="user.url">
          <img :src="user.avatar_url">
        </a>
        <span>{{user.name}}</span>
      </div>
    </div>
  </div>
</template>

<script>
import PubSub from "pubsub-js"
import axios from "axios"
import { constants } from 'crypto';
export default {
  name:"Main",
  data() {
    return {
      loading:false,
      errormsg:"",
      users:null
    }
  },
  mounted(){
    // 订阅消息
    PubSub.subscribe("search",(msg,searchName)=>{
      const url = `https://api.github.com/search/users?q=${searchName}`;
      // 更新状态
      // 这个时候在获取数据中
      this.loading = true;
      this.users = null;
      this.errormsg = "";
      axios.get(url).then((response)=>{
        // console.log(response);
        // console.log(response.data);
        // console.log(response.data.items);
        const result = response.data;
        const users = result.items.map(item => ({

          url: item.html_url,
          avatar_url: item.avatar_url,
          name: item.login 
        }))
        // 更新请求成功的状态
        this.loading=false;
        this.users=users;
      }).catch(error=>{
         this.loading=false
        this.errormsg='搜索失败'
      })
    })
    
  }//mouted
}
</script>

<style scoped>
.row{
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}
.item{
  float: left;
  width: 100px;
  text-align: center;
  margin: 0 10px;
}
.row a img{
  width: 100px;
  height: 100px;
}
</style>

```

1. pub-sub库的使用。
   项目中我们头部header需要向后台发送关键字，后台根据得到的关键字进行相应的操作，返回项目需要的数据。main主体区域中需要利用后台返回的数据，进行页面的渲染，main区域中必定会利用header中提供的关键字发送ajax请求，所以这就牵扯到组件之间的通信问题，pubsub-js就是用来实现组件之间通信的。兄弟组件之间通信如果利用props属性，需要借助父组件来实现，pubsub跨越组件之间的关系阶层进行通信。pubsub-js也就是我们所说的订阅消息和发布消息，***订阅消息可以理解为事件的监听，发布消息可以理解为触发事件***。我们在header中点击搜索，之后会通知main区域向后台发送Ajax请求，所以我们在header中发布消息，main中订阅消息。
   ![1565174213326](C:\Users\mengjietsai\AppData\Roaming\Typora\typora-user-images\1565174213326.png)

利用pubsub，首先需要导入这个包。search按钮点击的时候，我们可以在按钮点击的回调函数里去发布消息。发布消息是`PubSub.publish()`方法，这里需要提供两个参数，"发布的消息名"，"提供给订阅者的参数"，这里的参数是输入框的关键字。

main区域在组件的生命周期函数mounted（页面加载完成）中订阅消息。订阅消息是`PubSub.subscribe()`方法，这里接受两个参数，"发布的消息名"，"事件的监听函数"，这里事件的监听函数需要两个参数：一个是msg(发布的消息名，无用)，一个是searchName（发布者传来的参数）。我们在事件的监听里面去发送Ajax请求，更新页面。
![1565174242092](C:\Users\mengjietsai\AppData\Roaming\Typora\typora-user-images\1565174242092.png)

实现效果

![img](https://img2018.cnblogs.com/blog/1632878/201903/1632878-20190321110201365-302227376.gif)



**参考博客园的 pubdreamcc https://home.cnblogs.com/u/dreamcc/用户**

