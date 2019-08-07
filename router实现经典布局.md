```html
<div class="app">
    <router-view></router-view>
    <div class="container">
        <router-view name="left"></router-view>
        <router-view name="main"></router-view>
    </div>
</div>
```

```javascript
let head = {
    template: "<h2 class='head'>head</h2>"
};
let leftBox = {
    template: "<h2 class='left'>leftBox</h2>"
};
let mainBox = {
    template: "<h2 class='main'>mainBox</h2>"
};
let routes = [{
    path: "/",
    components: {
        "default": head,
        "left": leftBox,
        "main": mainBox,
    }
}];
let router = new VueRouter({
    routes,
});
var vm = new Vue({
    el: ".app",
    router,
});
```

```css
* {
    margin: 0;
}

.head {
    background-color: hotpink;
    height: 100px;
}

.container {
    display: flex;
}

.left {
    flex: 2;
    background-color: green;
    height: 600px
}

.main {
    flex: 10;
    background-color: skyblue;

}
```

![1564884608200](C:\Users\mengjietsai\AppData\Roaming\Typora\typora-user-images\1564884608200.png)